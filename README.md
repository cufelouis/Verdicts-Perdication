### Verdicts-Prediction

-------

#### Verdicts prediction based on past trial data

Notes on 07/12/2019

- We have realized the analysis in extracting judegs, juries and clerks, including their employing status (实习/代理).
- We simply extracted the parties in every case, ready for later analysis. 
- Courts level and region code information is extracted from government website, for comparisons.



#### MySQL Connector

find the code block below header **Load data from mySql database**

```python
connection = mysql.connector.connect(host ='localhost', database = 'Judgements', user = 'root', password = '123456')
```

modify the query line for different inputs.



#### Multithreads

The functionality was newly added and under test.

We decided the number of threads based on the setting on my testing computer.

To adjust number of threads to maxmize effeciency for your own computer:

1. find the function **divide_dataframe**, change the number $4$ to the number of cores you computer owns.
2. for every threads creating block: increase the number of threads created.

*Example*:

for a computer with 8 cores, the process of extracting parties should be changed to:

```python
input_dataframe_list = divide_dataframe(df)
try:
    thread1 = extract_parties_threads(1, "thread-1", input_dataframe_list[0])
    thread2 = extract_parties_threads(2, "thread-2", input_dataframe_list[1])
    thread3 = extract_parties_threads(3, "thread-3", input_dataframe_list[2])
    thread4 = extract_parties_threads(4, "thread-4", input_dataframe_list[3])
    thread5 = extract_parties_threads(5, "thread-5", input_dataframe_list[4])
    thread6 = extract_parties_threads(6, "thread-6", input_dataframe_list[5])
    thread7 = extract_parties_threads(7, "thread-7", input_dataframe_list[6])
    thread8 = extract_parties_threads(8, "thread-8", input_dataframe_list[7])
    
    # Start new Threads
    thread1.start()
    thread2.start()
    thread3.start()
    thread4.start()
    thread5.start()
    thread6.start()
    thread7.start()
    thread8.start()

    # Add threads to thread list
    threads.append(thread1)
    threads.append(thread2)
    threads.append(thread3)
    threads.append(thread4)
    threads.append(thread5)
    threads.append(thread6)
    threads.append(thread7)
    threads.append(thread8)
    
    # Wait for all threads to complete
    for t in threads:
        t.join()
    print ("Exiting Main Thread")
except:
    print("Error: unable to start thread")
finally:
    df = input_dataframe_list[0].append(input_dataframe_list[1]).append(input_dataframe_list[2]).append(input_dataframe_list[3]).append(input_dataframe_list[4]).append(input_dataframe_list[5]).append(input_dataframe_list[6]).append(input_data_list[7])
```



#### Issues

- Accuracy of courts matching remains to be improved: 法院名称记录不规范（省市县/区+法院名称 或 区/县法院名称），行政区划调整难以被追踪.
- Region code matching is facing the same problem, so it was eliminate from current program.
- Multi-thread processing may still not be powerful enough, needs to be improved.