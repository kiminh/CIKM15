# The datasets and programs for our CIKM15 paper

Please contact the following author if you need further classification.  
Author:  Hongwei Liang  
Contact: hit.henryleung@gmail.com 


## 1.SUMMARY

This folder includes the datasets and the executive files of the PDFS algorithm for the following paper:  

Zhang, Chenyi, Hongwei Liang, Ke Wang, and Jianling Sun. "Personalized Trip Recommendation with POI Availability and Uncertain Traveling Time." In Proceedings of the 24th ACM International on Conference on Information and Knowledge Management, pp. 911-920. ACM, 2015.

```
@inproceedings{zhang2015personalized,
  title={Personalized Trip Recommendation with POI Availability and Uncertain Traveling Time},
  author={Zhang, Chenyi and Liang, Hongwei and Wang, Ke and Sun, Jianling},
  booktitle={Proceedings of the 24th ACM International on Conference on Information and Knowledge Management},
  pages={911--920},
  year={2015},
  organization={ACM}
}
```

Please cite this work if either the datasets or the executive files are used by your works.

Datasets: they are in the folder "data".   
Programs: the executive files for the platforms Linux, mac and windows are in the root directory. The file "libstdc++-6.dll" is a library maybe required by PDFSLogNormal_windows.exe. The outputs of the programs will be stored in the folder "res-log".  

Currently, we have not released the source code for the executive files. If you are really interested in developing your algorithm based on our algorithms, please contact us in private.  

See the description of the datasets and the usage of the executive files below.  

##  2.DESCRIPTION OF THE DATASETS

There are 3 check-in datasets: LA(Los Angeles), NY(New York city) and PX(Phoenix). LA and NY are from Foursquare and PX is from Yelp.
For each dataset XX, 5 files are related: XX.txt, XX_top.txt, XX_coor.txt, XX_op.txt and XX_meta.txt. We take LA as an example to introduce each file. For statistics and detailed description, please refer to the Section 7 of the paper.

**LA.txt**: this is the rating data for 1184 users (lines) and 589 POIs(columns separated by "\t"), generated by the Feature centric collaborative filtering (FCF) method.   
The original rating scale for Foursquare datasets is 0/1, but the generated ratings by FCF maybe out of the range [0,1]. The original rating scale for Yelp datasets is [1,5]. LA.txt stores the intermediate results and it is not used by the programs.

**LA_top.txt**: this file is produced based on LA.txt by selecting the top 150 unvisited POIs ranked by their estimated ratings. The ratings for other POIs are set to 0. Please note that we used the rating directly generated by FCF and did not restrict the rating range to be original range. Since we select top 150, most of the 150 will be 1 if we restrict the range, which will make the total rating of many trips no difference. LA_top.txt serves as a input of the programs.

**LA_coor.txt**: it includes the geographic positions for the 589 POIs. Each line of this file is the "latitude \t longitude".

**LA_op.txt**: it indicates the opening hours for each of the 589 POIs. The format of each line is "openingTime \t closingTime". The default starting time of a day is "8am", therefore, "0" indicates "8am", "1" indicates "9am" and so on. 

**LA_mata.txt**: this file specifies some hard parameters for the programs. It has only one line and the format is "#ofUsers \t #ofPOIs \t latitudeOfSource \t longitudeOfSource".   
"#ofUsers" is the total number of users that the program will recommend trips for. The default value is 100, which means the first 100 users, and it can be adjusted in the range of [1, totalNumberOfUsers].  
"#ofPOIs" is fixed.    
The latitude and longitude of source indicates the default starting and ending location of the trip for each user. It can be modified as long as it is still in the same city. The default setting for LA is Hollywood, for NY is Central Park and for PX is Central City.




##  3.HOW TO RUN

Four parameters are required by the programs:   
i. **dataset indicator**: such as "LA", "NY" or "PX". People can create their own dataset following the format of the existing datasets. For example, if you create a new dataset "XX", you need the 4 files XX_top.txt, XX_coor.txt, XX_op.txt and XX_meta.txt, then you can use "XX" as a parameter of the programs.

ii. **time budget**: 50 means 5 hours, 60 means 6 hours and so on.

iii. **departure time**: 0 means 8am, 10 means 9am, 20 means 10am and so on.

iv. **completion probability threshold**: the range is in [0,1]. See Section 3.3 in the paper. When "0" is specified, it applies the fixed traveling time model (Section 7.3), otherwise, it applies the uncertain traveling time model.

Use command line to run the algorithms. Suppose all the files are in the folder "fd", the examples of running the programs are:

**For Linux platform**  
(1)cd fd  
(2)./PDFSLogNormal_linux LA 70 0 0.8  
 
**For Mac platform**  
(1)cd fd  
(2)./PDFSLogNormal_mac NY 50 0 0  

**For Windows platform**  
(1)cd fd  
(2)PDFSLogNormal_windows.exe LA 50 10 0  




## 4. OUTPUT
When running the programs, they will print the following information to the console:

User: 1, time budget: 50, T0: 0, P_threshold: 0  
Program running time: 157 ms  
Optimal path: +0+428+579+368+0  
Total preference: 2.94305  
Overall time used: 50  
....

"Program running time" is measuring the time of recommending the optimal trip, the time of reading data is not counted.  
"Optimal path: +0+428+579+368+0" means it is a round trip 0->428->579->368->0, where 0 is the source/destination and other numbers are the IDs of the POIs.  
"Total preference" is the total rating obtained by visiting the POIs in the trip.  
"Overall time used" indicates the real time the optimal path costs.  

The outputs are also written into a file under the "res-log" directory.
