---
description: >-
  This program includes a macro that generates a randomization list for the
  study according to the specifications given by the macro parameters (e.g.
  block size). 该程序包括一个宏，该宏根据宏参数给定的规范（例如块大小）为研究生成随机列表。
---

# Block Randomization



## Beschreibung

|             |                                                                                                                                                                                                                           |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Num_Obs     | Number of required observations                                                                                                                                                                                           |
| Ratio       | <p>Randomization ratio of the different treatment arms.</p><p>Numbers have to be separated by ":". E.g. 3:2:4.</p>                                                                                                        |
| Seed        | <p>Number, which will be used to generate random numbers.</p><p>For 0 it is not possible to reproduce the randomization. For all other numbers you will always get the same result, obtained with exactly this number</p> |
| Num_Sites   | Number of Sites                                                                                                                                                                                                           |
| Name_Treats | Name of the treatment arms. Names have to be separated by "/"                                                                                                                                                             |
| Output      | Output name                                                                                                                                                                                                               |



## Generate Block Randomization 

```
*** With this macro it is possible to take account of an infinite number of sites;
*** Required input:		  - Num_Obs 		 Number of required observations
					 	  - Ratio   		 Randomization ratio of the different treatment arms.
						 				     Numbers have to be separated by ":". E.g. 3:2:4.
						  - Seed	  		 Number, which will be used to generate random numbers.
								   		     For 0 it is not possible to reproduce the randomization. For all other
								   	 	     numbers you will always get the same result, obtained with exactly this number
						  - Num_Sites		(Number of Sites)
						  - Name_Treats		 Name of the treatment arms. Names have to be separated by "/"
						  - Output			 Output name;

proc datasets library=work kill nolist; run; quit;

option spool; 

dm log 'clear';
dm output 'clear';

%global today;
%let today=%sysfunc(today(),date9.); 
%put &today;
	
%let programme_name  = AMT101204_RandomList_V01_0_0.sas;
%let sponsor		 = Applied Molecular Transport Inc.;
%let studyacronym    = AMT-101-204;
%let studyfolder     = Z:\STUDIES\Charité Research Organisation\AMT_101_204;
libname ran_path "&studyfolder.\WORKSPACE\07_STATISTIC\07_Randomization\04_Randomlist_final";
*libname rando  "&studyfolder.\WORKSPACE\07_STATISTIC\07_Randomization\04_Randomlist_final";

%macro BlockRandom (num_obs, 
					block_length,
					ratio, 
					seed, 
					name_treats,
					num_sites, 
					output); 
			
	proc datasets library=work;
		delete randomlist;
	run;

	data &output;

		num_treats=count(&name_treats,"/")+1; 		* Number of treatments. The count-statement counts
												      the number of "/" in the string. As there is always one "/" less than
												      treatments, the addition of 1 is needed;

		sum_ratio = 0;										* Sum of all digits in variable Ratio.The 
															  scan-statement extracts the digit before the i-th ":".If Act_Treat
															  is greater than the number of ":" the last digit is extracted;
		do act_treat=1 to num_treats;
				act_digit = input(scan(&ratio, act_treat, ":"),5.);
				sum_ratio = sum_ratio + act_digit;
		end;

		block_length = sum_ratio * ceil(&block_length / sum_ratio); 
		num_obs = block_length*ceil(&num_obs / block_length) /&num_sites;
																						*The number of observations has to be a
																							 multiple of Num_Required_Groups,
																							 otherwise the list contains less
																							 than needed and is unbalanced;	
		req_num_blocks= ceil(num_obs / block_length);		*Required number of blocks;

		obs_per_block = num_obs /req_num_blocks;			

		do site_num=1 to &num_sites;	
			do block=1 to req_num_blocks;	
				do act_treat = 1 to num_treats;					* Loop over each treatment;
					obs_per_unit = obs_per_block/sum_ratio;		* Obs_per_Unit is the number, required for one unit 
																  of the ratio;
					treat_digit  = input(scan(&ratio, act_treat, ":"), 5.);	
					required_individuals = obs_per_unit * treat_digit; 	 * Required_Individuals is the number of individuals, needed 
															     		   for the treatment, which is actually considered;
					do individual = 1 to required_individuals;		* Generation of the required number of individuals and
																 	  assignment of the actual treatment name via scan-statement;
						treatment = strip(scan(&name_treats, act_treat, "/"));
						keep site_num block treatment obs_per_block;  
	 		   			output;
					end;
				end;
			end;
		end;
	run;
	

*** Each entry in the dataset Randomlist receives a random number between 0 and 1. The number is descended from a uniform
	distribution;
	data &output; 
		set &output;
		 u = ranuni(&seed);
	run;	


*** After sorting the entries by the random number U, the order of the entries is random;
	proc sort data=&output;
		by site_num block u;
	run;

	data &output; 
		length rando_num $50.;
		set &output;
		rando_num = strip(put(_n_, z3.0));
		drop u;
	run;	

%mend BlockRandom;



*** Create the randomization list for the first IMP shipment;
	*** define block size and randomization seed for the first list;
	%let block_length   = 4;
	%let seed			= 566384;

	%BlockRandom  (num_obs=8, 								
				   block_length=&block_length, 
				   ratio="1:1", 
				   seed=&seed, 
				   num_sites=1,
				   name_treats="Verum/ Placebo", 
				   output=AMT101204_randomlist_D01);
```

![](broken-reference)

## Format Randomlist

```
*** Update and reformat the list;
data  AMT101204_randomlist_D02;
	set AMT101204_randomlist_D01 (in=initial  rename=(rando_num = rando_pre))
		AMT101204_randomlist_D01 (in=replacer rename=(rando_num = rando_pre));
	*** update randomization lists for replacer;
	if replacer then rando_pre = "1" || strip(substr(rando_pre,2,2));
	*** update randomization number to match with expectation;
	rando_num = "204-4163-" || strip(vvalue(rando_pre));
	*** annotate initial vs. replacer list;
	length list $20.;
	if initial then list = "Initial";
	else if replacer then list = "Replacer";
	*** rename Verum to study drug name;
	if Treatment = "Verum" then Treatment = "AMT-101";
	*** annotate general information;
	Study = "AMT-101-204";
	*** clean dataset;
	drop rando_pre obs_per_block block site_num;
run;

*** Update randomlist for merging;
proc sort data=AMT101204_randomlist_D02; by treatment list rando_num; run;
data AMT101204_randomlist_D02a;
	set AMT101204_randomlist_D02;
	by treatment list rando_num;
	*** create merging variable;
	retain patient 0;
	if first.list then patient = 1;
	else if not first.list then patient = patient +1;
run;

*** Obtain information on kit number;
*** Each patient receives three (3) kits, since medication in one kit only lasts for 1/3 of the duration of the trial;
*** Information regarding the kits that are to be handed out should not be provided by visit, but in total per patient (one entry with 3 kit numbers);
proc import out = AMT101204_kit_list
			datafile = "Z:\STUDIES\Charité_Research_Organisation\AMT_101_204\WORKSPACE\07_STATISTIC\07_Randomization\08_Unblinding\AMT_101_204_kit_list_unblinded.csv"
			dbms = csv
			replace;
	getnames = yes;
	delimiter = ";";
	guessingrows = 10000;
run;

*** Prepare information for merging;
proc sort data=AMT101204_kit_list; by Treatment_Group; run;
data AMT101204_randomlist_D03_rand AMT101204_randomlist_D03_backup;
	length kit_numbers $30.;
	set AMT101204_kit_list (keep = Treatment_Group Lot_Serial_Number);
	by Treatment_Group;
	*** delete empty row;
	if treatment_group eq "" then delete;
	*** merge three kit numbers into one item;
		*** device counter for kit numbers;
		retain count 0;
		if first.treatment_group then count = 1;
		else count = count+1;
		if count gt 3 then count=count-3;
		*** retain and merge kit numbers;
		retain kit_numbers;
		if count eq 1 then kit_numbers = strip(vvalue(lot_serial_number));
		else if count gt 1 then kit_numbers = strip(vvalue(kit_numbers)) || ", " || strip(vvalue(lot_serial_number));
	*** annotate patients and ensure that there are two lists: One for kits to be randomized, and one for kits to be used as back-ups;
	*** for the time being (first list / V01), 4 patients per treatment and 3 kits as back-up are allocated;
		retain patient 0;
		if first.treatment_group then patient = 1;
		if not first.treatment_group and count eq 1 then patient = patient +1;
	*** annotate treatment group for merging;
	if treatment_group eq "3MG" then treatment = "AMT-101";
	else if treatment_group eq "PBO" then treatment = "Placebo";
	*** clean data set;
	keep treatment patient kit_numbers lot_serial_number count;
	*** output patients for randomization;
	if count eq 3 and patient in (1,2,3,4) then output AMT101204_randomlist_D03_rand;
	else if patient eq 5 then output AMT101204_randomlist_D03_backup;
run;

*** Merge information for patients with randomization list;
proc sort data=AMT101204_randomlist_D02a; 		by treatment patient; run;
proc sort data=AMT101204_randomlist_D03_rand; 	by treatment patient; run;
data AMT101204_randomlist_D04;
	merge 	AMT101204_randomlist_D02a
			AMT101204_randomlist_D03_rand;
	by treatment patient;
	*** clean dataset;
	keep treatment rando_num list kit_numbers study;
run;

*** Get data in original order (by randomization number);
proc sort data=AMT101204_randomlist_D04; by rando_num; run;

*** Save random list permanent;
DATA ran_path.AMT101204_randomlist_D04;
		set Amt101204_randomlist_d04;
RUN;

```



![](broken-reference)

## Proc Template RandoList Style

```
***************************************************************************************************************;
*** Define output style by proc template ;
***************************************************************************************************************;

proc template;
	define style GCPS_style_RandoList;
		*** reduce standard space before and after table;
		style parskip / fontsize = 1pt;

		*** add continued-tag, if the table is too long for one page;
		style continued from continued /
			pretext="(continued)"
			font=("Lucida Sans", 9pt);

		*** define style for data within the output document;
		*style titleAndNoteContainer / outputwidth = _undef_;
		style data / foreground= black 
			 font_face ='Lucida Sans'
			 font_weight = medium
			 font_size = 9pt 
			 protectspecialchars=off
			 just=left
			 asis=on
		; 
		* asis is used for indent variable entries;
		
		*** define style for header of the document;
		style header /
			protectspecialchars=off
			font_face = 'Lucida Sans' 
			font_weight = medium
			font_size = 9pt
			just=left
			asis=on
			backgroundcolor=white
		;
		style PageNo from TitlesAndFooters / 
            font_face= 'Lucida Sans'  
            cellpadding = 0
            cellspacing = 0
            just=left 
            vjust=b; 
		*** define style for table body;
		style Table /
			cellspacing=0pt
			cellpadding=1
			padding=2 cm
			frame=hsides
			just=left
			rules=groups;
			
		
		*style systemtitle /
			*font_face = 'Lucida Sans' 
			*font_weight = medium
			*font_size = 11pt
			*protectspecialchars=off
			*just=left;
		
		*** define style for footer of the document;
		style systemfooter /
			font_face = 'Lucida Sans' 
			font_weight = medium
			font_size = 10pt
			just=left;
		
		*** define style for text outside of tables;
	   style usertext from usertext /
			font_face = 'Lucida Sans';
	
		
		*** define page margins of the document;
		style body "Set page margins" /
			bottommargin = 0.1in
			topmargin = 0.1in
			rightmargin = 0.7in
			leftmargin = 0.7in;  
	   end;                                                                       
run;                   
quit;

```

## Proc Report Output

```
ods exclude none;
 
	%include "Z:\STATISTICS\08_Team\ZBA\Randomization\GCPS_Style_RandoList_V01.sas";
	option orientation=portrait  papersize=32767 linesize=256;;
	ods tagsets.rtf file="Z:\STATISTICS\08_Team\ZBA\Randomization\AMT101204_RandomizationList_V01_&today..rtf"
			style=GCPS_style_RandoList
			  options( contents='no' 
					   continue_tag='yes')
			  startpage=yes uniform;
				


	ods escapechar="^";	
		title1 j=r '^S={preimage="Z:\STATISTICS\logo.jpg"}';
		title2 font='Arial' h=9pt  
					j=l '^R/RTF"{'&sponsor' \brdrb\brdrs\brdrw10\brsp10\par}"'
					j=c '^R/RTF"{Randomization List 1 \brdrb\brdrs\brdrw10\brsp10\par}"'
					j=r '^R/RTF"{\brdrb\brdrs\brdrw10\brsp10\par}"'	;
		footnote1 font='Lucida Sans' h=1pt j=l '^R/RTF"{\rtf1\ansi\deff0{\fonttbl  {\f0 Arial;}}
		 {\footer\pard\qr\plain\f0\afs20{ \brdrb\brdrs\brdrw10\brsp10\par' &studyacronym'                                  '&today'                                 Page \chpgn  of{ \field{\*\fldinst NUMPAGES}}}}}"'; 

	ODS tagsets.rtf  TEXT="^R/RTF'\fs20\b' Generated by program:		        		^R/RTF'\tab\tab\b' ^_^_Used random seed:";
	ODS tagsets.rtf  TEXT="^R/RTF'\fs20' &programme_name                    ^R/RTF'\tab  ' &seed^n^n";

	options nodate nonumber nocenter;
	ods exclude none;
	ods tagsets.rtf tablerows=44 ;

	proc report data=AMT101204_randomlist_D04 headline headskip nowd split="|" missing nocenter;
		columns (study list rando_num	kit_numbers);
		define study				/ "^R/RTF'\brdrb\brdrs\b\ql' Study^n"    		    	order order=internal style(column)=[cellwidth=3.5 cm];*style(column)=[cellwidth=3.8 cm];
		*define target_flag			/ ""													order order=internal noprint;
		define list				/ "^R/RTF'\brdrb\brdrs\b\ql' List^n"					order style(column)=[cellwidth=3.4 cm];*style(column)=[cellwidth=2.7 cm];
		*define Cohort				/ "^R/RTF'\brdrb\brdrs\b\ql' Cohort^n"					style(column)=[cellwidth=2.5 cm];
		*define rando_info			/ "^R/RTF'\brdrb\brdrs\b\ql' Randomization^ninfo"  		style(column)=[cellwidth=2.9 cm];*style(column)=[cellwidth=3.9 cm];
		define rando_num			/ "^R/RTF'\brdrb\brdrs\b\ql' Randomization^nnumber"  	style(column)=[cellwidth=4 cm];*style(column)=[cellwidth=2.9 cm];
		define kit_numbers			/ "^R/RTF'\brdrb\brdrs\b\ql' Kit ^nnumbers"	   		style(column)=[cellwidth=4.8 cm];*style(column)=[cellwidth=2.0 cm];

		break after list / page;

	run;

	title;footnote;

	ods tagsets.rtf close;
	ods _all_ close;

***************** back up Kits in CSV format;
proc export data=Amt101204_randomlist_d03_backup (keep=Lot_Serial_Number treatment)
outfile='Z:\STUDIES\Charité Research Organisation\AMT_101_204\WORKSPACE\07_STATISTIC\07_Randomization\04_Randomlist_final\AMT101204_BackupList_V01.csv' 
dbms=csv
replace;
run;

```

![](broken-reference)
