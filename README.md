# Big Data Analysis to find the optimal performance characteristics. 

## Skills: Big Data, Hadoop, MapReduce, Java, Tableau

In this project we will analyse the optimal performance characteristics relating to compression, intermediate compression, combiners, block size and the number of reducers. The aim of the project is to test these characteristics and find the optimal solution to improve the performance of the MapReduce tasks. 

The dataset used for this project is the weather dataset from ncdc. The MapReduce tasks were run on the different datasets which were with the above characteristics and the performance times were analysed.

### Test 1

First we analyse the number of reducers and the role of the reducers with regards to the performance. For this test we do not use a combiner or intermediate compression. The first test has one reducer and we perform the same test with 2,4 and 8 reducers one after the other. The graph given below shows the performance of the subsequent MapReduce tasks. 

![test1](https://github.com/vchandrasekaran/big-data-analysis/blob/master/Images/test1.png)

The above graph is plotted with the elapsed time in minutes on the y axis and the file application ids are given on the x axis. The file type is also mentioned on the x axis headings for better clarity with analysis regarding the file types and its influence in obtaining the process timings. The colours of the bars indicate the number of reducers used for that job and is as follows: Green for 1 reducer, pink for 2 reducers, orange for 4 reducers and red for 8 reducers.

On the first look we can see that the number of reducers play a huge role in the process times in the test. We can see that the reduce times for the given tasks reduces significantly as the number of reducers increase. Also we notice that the file format also has an impact on the process times as the gz files are run faster and the bz2 files are next to the gz files. The text files are run comparatively slower than the other two kinds of files.

We can see that the increase in reducers reduces the process times. 

### Test 2

In the second test we add on more characteristics to the MapReduce tasks. Along with the number of reducers the combiner and intermediate compression is added. 

The MapReduce jobs are performed with 1 reducer first, 2 reducers next and then with 4 reducers and 8 reducers respectively. For every task we ensure that the combiner is added along with the compression. The graph given below shows the performance of these tasks. 

![test2](https://github.com/vchandrasekaran/Big-Data-Analysis/blob/master/Images/Test2.png)

The above graph is plotted with the elapsed time in minutes on the y axis and the file application ids are given on the x axis. The file type is also mentioned on the x axis headings for better clarity with analysis regarding the file types and its influence in obtaining the process timings. The colours of the bars indicate the number of reducers used for that particular job and is as follows: Green for 1 reducer, pink for 2 reducers, orange for 4 reducers and red for 8 reducers.

We can see that the combiners and the compression is used. The graph shows that with more reducers the process time is increased across all the file types. The compression is used on gzip compressed file which does not support splitting. The map reduce will not try to split the gz file since it is not supported. In this case a single map will process the HDFS blocks and will not be local to the map. Due to this the time taken for execution will be longer compared to the other formats. The bz2 files should ideally be supported by the compression and is suited for map reduce jobs.

Also increasing the number of reducers may not always be a good tactic. In this particular test more time is wasted on shuffle and merging as a result of more reducers used on smaller file sizes. More spill files are created due to the small file sizes which hinders the performance in this case. The ideal number of reducers are to be used to achieve performance.

### Test 3

For this test we ignore the intermediate compression and check the performance of the MapReduce tasks. All the other factors are like the previous tests. 

![test3](https://github.com/vchandrasekaran/Big-Data-Analysis/blob/master/Images/Test3.png)

The above graph is plotted with the elapsed time in minutes on the y axis and the file application ids are given on the x axis. The file type is also mentioned on the x axis headings for better clarity with analysis regarding the file types and its influence in obtaining the process timings. The colours of the bars indicate the number of reducers used for that job and is as follows: Green for 1 reducer, pink for 2 reducers, orange for 4 reducers and red for 8 reducers.

The graph gives us a straightforward picture on how effective combiners can be along with the reducers. As the number of reducers increase with combiners there is a fall in the execution times. The gz file with 8 reducers executes in about 5 minutes when compared to the text file with one reducer and combiner. Therefore the gz file format can also be concluded as an optimization when used with more reducers and a combiner. More reducers means more parallelism and the combiners reduces the average shuffle times as well.

### Test 4

In this test we are challenged with bad records. To encounter this we will have to modify our code to handle bad records and we will also add a counter at the end of the file to show the number of bad records. We will repeat test 1 where in we used the reducers to determine the performance. In the later tests we will add on the other characteristics and test the performance. 


![test4](https://github.com/vchandrasekaran/Big-Data-Analysis/blob/master/Images/test4.png)

The above graph is plotted with the elapsed time in minutes on the y axis and the file application ids are given on the x axis. The file type is also mentioned on the x axis headings for better clarity with analysis regarding the file types and its influence in obtaining the process timings. The colours of the bars indicate the number of reducers used for that particular job and is as follows: Green for 1 reducer, pink for 2 reducers, orange for 4 reducers and red for 8 reducers.

We can see that a larger file is used here which is directly proportional to the large execution times. The combiners are not used which will not support any of the large number of reducers used here. On the whole we are just running a simple map reduce job and we can say that based on the bz files the larger the number of reducers the faster is the job execution timings. Here the ideal number of reducers to be used can be chosen to be 4 as this has the least execution times across all the file formats based on the size of the file.

### Test 5

In this test we will have bad records and will we handle it in the similar to the previous test. We will be using a larger dataset for this test and will check the characteristics. We will test the number of reducers along with the use of the combiner and the intermediate compression. 

![test5](https://github.com/vchandrasekaran/Big-Data-Analysis/blob/master/Images/test5.png)

The above graph is plotted with the elapsed time in minutes on the y axis and the file application ids are given on the x axis. The file type is also mentioned on the x axis headings for better clarity with analysis regarding the file types and its influence in obtaining the process timings. The colours of the bars indicate the number of reducers used for that particular job and is as follows: Green for 1 reducer, pink for 2 reducers, orange for 4 reducers and red for 8 reducers.

The compression and combiner is used here. We can see that as we increase the number of reducers for the jobs across all file types there is a decrease in the job execution timings. On large files such as the dataset used here for this analysis we can conclude that the more number of reducers we use with compression and combiners we will get faster results. Another interesting fact here is that with two reducers we seem to get optimal performance for all the three file types. The combiners are responsible for reducing the average shuffle times for more reducers used. The compression works effectively on the bz2 file and is comparatively weaker for the gz file as there is a rise in the execution times slightly. Choosing two reducers for this file type can help with performance due to the size of the file and the splits used by the map reduce job. Since a map job can perform on one fille without splitting into multiple smaller files this seems to be the optimal tuned tactic.

### Test 6

We will repeat one of the previous tests with a larger dataset. We will not be using the intermediate compression for any of the tests. This would give us a better picture of the role of the intermediate compression on large datasets. However we will be using the combiners. 

![test6](https://github.com/vchandrasekaran/Big-Data-Analysis/blob/master/Images/test6.png)

The above graph is plotted with the elapsed time in minutes on the y axis and the file application ids are given on the x axis. The file type is also mentioned on the x axis headings for better clarity with analysis regarding the file types and its influence in obtaining the process timings. The colours of the bars indicate the number of reducers used for that particular job and is as follows: Green for 1 reducer, pink for 2 reducers, orange for 4 reducers and red for 8 reducers.

The above graph shows the impact of the combiner on the reducers clearly. We can observe that the combiners are playing their role with the reduced process times for the records with more reducers. This can be clearly observed with the text files as the reducers increase there is a fall in the execution times due to the effect of the combiner. Another point to observe is the lack of the compression has played a part on the execution times of the bz2 and the gz files. When compared with the previous result of test 5 we can see that the job execution times in this test have gone down without the usage of the compression and only the combiners. this shows that we should not be using the compression always along with the combiners as it is not necessarily an optimal idea.

### Test 7 

In this test the lack of combiner and the intermediate compression for the large datasets can be analysed. We will be performing the test like the first test. Only difference is that this will be a larger dataset. 

![test7](https://github.com/vchandrasekaran/Big-Data-Analysis/blob/master/Images/test7.png)

The above graph is plotted with the elapsed time in minutes on the y axis and the file application ids are given on the x axis. The file type is also mentioned on the x axis headings for better clarity with analysis regarding the file types and its influence in obtaining the process timings. The colours of the bars indicate the number of reducers used for that particular job and is as follows: Green for 1 reducer, pink for 2 reducers, orange for 4 reducers and red for 8 reducers.

The above graph is given for the tests with no combiner and compression used and just the reducers and the file types are a criteria. The graph obtained is like the graph obtained from test 1 and we can say that the time difference is purely because of a larger file size used in this test. an important observation here is the bz2 files are more effective with 2 or 4 reducers used for larger files like this one. As and when the size of the file increases we can see that the number of reducers we choose must be more precise for optimal performance.

### Test 8

In this test the effect of combiner and the intermediate compression for the large datasets can be analysed. We will be performing the test like the second test. Only difference is that this will be a larger dataset. 

![test8](https://github.com/vchandrasekaran/Big-Data-Analysis/blob/master/Images/test8.png)

The above graph is plotted with the elapsed time in minutes on the y axis and the file application ids are given on the x axis. The file type is also mentioned on the x axis headings for better clarity with analysis regarding the file types and its influence in obtaining the process timings. The colours of the bars indicate the number of reducers used for that particular job and is as follows: Green for 1 reducer, pink for 2 reducers, orange for 4 reducers and red for 8 reducers.

The above graph is obtained with the use of combiner and compression. The tests have a very simple graph that tells us that as and when we add more reducers the process times increase. So we can conclude that the number of reducers have to be lesser for large files with combiners and compression used. The bz2 files take nearly half the time taken by the raw text files. So a compressed bz2 file that supports splitting helps in the optimization for this dataset. As more reducers are used with compression there is no detail as to where the next split starts from so the time taken to process this by the reducers in mapping is considerable large.

### Test 9

In this test the effect of combiner and the lack of intermediate compression for the large datasets can be analysed. We will be performing the test like the third test. Only difference is that this will be a larger dataset. 

![test9](https://github.com/vchandrasekaran/Big-Data-Analysis/blob/master/Images/test9.png)

The above graph is plotted with the elapsed time in minutes on the y axis and the file application ids are given on the x axis. The file type is also mentioned on the x axis headings for better clarity with analysis regarding the file types and its influence in obtaining the process timings. The colours of the bars indicate the number of reducers used for that job and is as follows: Green for 1 reducer, pink for 2 reducers, orange for 4 reducers and red for 8 reducers.

The test shows the effect of the combiner on the datasets. Similar to the previous tests it is safe to assume that the combiners works effectively without compression with more number of reducers. This is like the test 5 result and the optimal number of reducers to be used here is 4 as it suits the file size used.

### Conclusion

When it comes to tuning a job there are several factors that come in the picture. The major factors that we have encountered here would be intermediate compression, compression, combiner usage, the default block sizes and the number of reducers. The role of each of these individual characteristics vary upon the file type and also the size of the file.

Number of reducers: It is always optimal to use more than a single reducer. The reduce tasks should run for at least 5 minutes and be able to produce a blocks worth of data to be effective. Using a single reducer will result in all the intermediate data flowing through one reducer and will increase the process time. Almost every time increasing the reducer will reduce the reduce phase due to more parallel tasks being executed. But increasing this number too high based on the file size can have adverse effects. This number should be chosen such that each reducer can produce at least one blocks worth of data.

When there is no combiner and compression used the reducer number is always a factor as the more number of reducers on larger file reduces the process time considerable. The reducers when used with combiners are most effective which can be seen in the tests with the combiners namely tests 3, 6 and 9. The adverse effects can be seen with reducers used in test 2 and 8. More reducers should be used with combiners and the file size should be considerably larger.

Combiners: The combiners are used to limit the data transferred between map and reduce tasks. It helps to optimize the performance of the job as seen by its usage in tests 3,6 and 9. The combiners are used to effectively reduce the shuffle and sort phases which can be seen here with larger files. However, we can see that the combiners used on small files with one reducer is just a waste of time. So, the combiners should be used with more reducers and when the file size is large.

Intermediate compression: The job execution time can almost always benefit from using map output compression. The mapper output is written to another disk and transferred across the network. Using a fast compressor can boost the performance as the volume of data to be transferred is reduced. The test 2 shows that the intermediate compression helps to reduce the process times of the job.

Compression: The compression of files helps in the optimal performance gain the bz2 compressed files are the most effective as it supports splitting. Let us go through the above tests to analyse the results. The smaller files used in the test 1 show that compression on the files reduces the process times. As we move further in test 2 we can see that the compression on the files with more reducers will increase the time for processing. As seen in test 3 we can see that compression with combiners and more number of reducers can be an optimal solution for smaller files. In tests 4 and 5 the compressed files with the effect of the reducers will increase the process timings. We can also see this effect of compression in test 8 where it is clear that compressed file with bz2 format will be very effective in reducing the process times.




























