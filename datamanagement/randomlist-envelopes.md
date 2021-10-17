# Randomlist Envelopes



```
*** Read Randomlist;
data Envelopes_D01;
    set amt101204_randomlist_d04;
	* set "Z:\STUDIES\Charité Research Organisation\AMT_101_204\WORKSPACE\07_STATISTIC\07_Randomization\04_Randomlist_final\amt101204_randomlist_d04";
run;

*** Combine Initial and Replacer;	
proc sort data=Envelopes_D01 (Where=(list="Initial")) OUT=Envelopes_D01_ini; by kit_numbers; run;

*** Rename random number for merging;
data Envelopes_D01_rep;
	set Envelopes_D01 (Where=(list="Replacer"));
	rename rando_num = rando_num_r;
	keep kit_numbers rando_num;
run;
proc sort data=Envelopes_D01_rep; by kit_numbers; run;

*** Write Initial and Replacer in next to each other;
data Envelopes_D02;
	length subject $200.;
	MERGE Envelopes_D01_ini 
		  Envelopes_D01_rep;
	by Kit_numbers;
	*** Create variable with both random number (replacer and initial);
	subject = strip(rando_num) || " / " || strip(rando_num_r);
	drop list;
RUN;

proc sort data=envelopes_d02; by rando_num; run;

*** Open output document;
	ods rtf file="Z:\STATISTICS\08_Team\ZBA\Randomization\AMT101204_Envelopes_20210125_final.rtf"
		style=gcpservice_style;
	options orientation=portrait nonumber nodate;

	ods escapechar="^";	
		
*** Macro for generation of the envelopes;
	%macro UnblindingEnvelope();
		%do i = 1 %to 8;

		*** General settings (i.a. page size and font);
			ods exclude none;
			ods graphics on;
			goptions gunit=cells HSIZE=16cm  VSIZE=24cm FTEXT='Calibri/bold'; 
			ods graphics / height=16cm width=24cm border=off;
			options pagesize=max;
			ods listing gpath="Z:\STATISTICS\08_Team\ZBA\Randomization";

		*** Macro variables for treatment and randomization number which should be printed on the envelopoes;
			%let treatment=;
			%let rando_num=;
			data _NULL_;
				SET Envelopes_D02; 
				if _n_ eq &i;
				call symput("treatment", strip(treatment));
				call symput("rando_num", strip(subject));
				

			run;

			*** Create an annotation dataset;
			%annomac; 
			data AMT_EmUnblEnvelopes_D01;
				length function color style $8. text $100.;

				*** Units for text formatting ('5' =  Percentage of procedure output area);
				retain xsys '5' ysys '5' hsys '5';/*when 'a' line 1 function 'label'*/;

				*** Coordinate system for correct formatting of the text (should be outcommented for final envelopes;
				%macro CoordinateSystem();
					%do count1=0 %to 100;
						 %label(0,  &count1,"&count1",BLACK,0,0,1.7,Calibri,0); * text size - 1.7 for PNG;
					%end;
					%do count2=0 %to 100 %by 2;
						 %label(&count2,  33,"&count2",BLACK,90,0,1.7,Calibri,0); 
					%end;

				%mend;
				*%CoordinateSystem;

	*** Lower page;
		*** Description: label (x-axis, y-axis, label, color, rotate (whole text),rotate (letters) ,text size, font, ?); 
				 %label(30, 12,"Open only in case of emergency!",RED,0,0,2.0,,0); 
				 %label(24, 8, "Emergency unblinding envelope number: ",BLACK,0,0,2.0,, 0); 
				 %label(33, 4, "&rando_num",BLACK,0,0,2.0,, 0); 

	*** Middle page;
				 %label(67, 42,"Study: AMT-101-204",BLACK,180,0,2.0,Calibri, 0); 
				 %label(70, 44.5,"Charité Research Organisation",BLACK,180,0,2.0,Calibri, 0); 
				 %label(80, 49.5,"Emergency unblinding envelope number: ",BLACK,180,0,2.0,Calibri, 0); 
				 %label(72, 52, "&rando_num",BLACK,180,0,2.0,Calibri, 0); 

	*** Upper page;
				 %label(22, 94,"Emergency unblinding envelope number:",BLACK,0,0,2.0,Calibri, 0); 
				 %label(22, 90,"&rando_num",BLACK,0,0,2.0,Calibri, 0); 

				* %label(22, 95,"Study: AMT-101-204",BLACK,0,0,2.0,Calibri, 0); 
				 %label(22, 86,"Treatment: &treatment",BLACK,0,0,2.0,, 0); 
			run; 


			/**********************************************************************************************************************************
			Output figure
			**********************************************************************************************************************************/
			proc gslide annotate=AMT_EmUnblEnvelopes_D01;
			run;
			quit; 

			footnote; 
			title;
			ods graphics off;
			ods exclude all;
		%end;	
	%mend UnblindingEnvelope;
	%UnblindingEnvelope;
	ods rtf close;
	ods _all_ close;

/**********************************************************************************************************************************
Tidy up
**********************************************************************************************************************************/
/*
*** Delete temporary datasets;
	proc delete data = Envelopes_d01; run;

*** Delete temporary macros;
	proc catalog cat=work.sasmacr;
		delete UnblindingEnvelope annomac CoordinateSystem / et=macro;
	quit;
*/
```





![](broken-reference)
