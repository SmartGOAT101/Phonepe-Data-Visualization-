# Phonepe_Pulse_Data_Visualization
Project-2 from GUVI | Data Visualization with Streamlit and Plotly library

# What is PhonePe Pulse?
 
 The PhonePe Pulse website showcases more than 2000+ Crore transactions by consumers on an interactive map of India. With over 45% market share, PhonePe's data is representative of the country's digital payment habits. The insights on the website and in the report have been drawn from two key sources - the entirety of PhonePe's transaction data combined with merchant and customer interviews. The report is available as a free download on the PhonePe Pulse website and GitHub.
        
# Want to see demo video of my project?-https://www.linkedin.com/posts/rahamaththulla-s-96121a165_hello-all-i-am-super-excited-to-share-activity-7053715279624765441-wa1G?utm_source=share&utm_medium=member_desktop       
        
# Libraries/Modules needed for the project!
     1.Plotly - (To plot and visualize the data)
     2.Pandas - (To Create a DataFrame with the scraped data)
     3.mysql.connector - (To store and retrieve the data)
     4.Streamlit - (To Create Graphical user Interface)
     5.json - (To load the json files)
     
 # Workflow
   # Step 1:
        Importing the Libraries:
                 Importing the libraries. As I have already mentioned above the list of libraries/modules needed for the project.
                 
                                       !pip install matplotlib
                                       !pip install pymysql
                                       !pip install sqlalchemy
                                       
 If the libraries are already installed then we have to import those into our script by mentioning the below codes. 
 
                  import sqlalchemy
                  import pymysql
                  from sqlalchemy import create_engine
                  from sqlalchemy import text
                  from pandas.io import sql
                  import mysql.connector as sql
                  import pandas as pd
                  import json
                  import plotly.express as px
                  import os
                  import streamlit as st
                  from PIL import Image
                  from sqlalchemy import create_engine, text 
                  import matplotlib.pyplot as plt
                  import mysql.connector as sql
# Step 2: 
     Data transformation:

        In this step the JSON files that are available in the folders are converted into the readeable and understandable DataFrame format by using the for loop and iterating file by file and then finally the DataFrame is created. In order to perform this step I've used os, json and pandas packages. And finally converted the dataframe into CSV file and storing in the local drive.

                    path1 = "Path of the JSON files"
                    agg_trans_list = os.listdir(path1)

       Give any column names that you want
       columns1 = {'State': [], 'Year': [], 'Quarter': [], 'Transaction_type': [], 'Transaction_count': [],'Transaction_amount': []}
       
              data_aggre_trans_path=(r"G:\guvi work\pulse-master\pulse-master\data\aggregated\transaction\country\india")
              common_data=os.listdir(data_aggre_trans_path)
              print(common_data)
              for i in common_data:
                   if i.isnumeric()==True:  
                       year=data_aggre_trans_path+'/'+i+'/'
                       json_path=os.listdir(year)
                       for onejson in json_path:
                          files=year+onejson
                          data=open(files,"r")
                          final_data=json.load(data)
                          for datas in final_data["data"]["transactionData"]:
                                 name=datas["name"]
                                 count=datas["paymentInstruments"][0]["count"]
                                 amount=datas['paymentInstruments'][0]["amount"]
                                 dic_yearbased["Transaction_type"].append(name)
                                 dic_yearbased["Transaction_count"].append(count)
                                 dic_yearbased["Transaction_amount"].append(amount)
                                 dic_yearbased["quater"].append(int(onejson.strip(".json")))
                                dic_yearbased["year"].append(i)
                               
     
     
     
    if i.isnumeric()==False:
        state=data_aggre_trans_path+"/"+i
        all_state=os.listdir(state)
       #print(all_state)
        for j in all_state:
            onebyone_state=state+"/"+j
            onebyone_state_year=os.listdir(onebyone_state)
        # print("onebyone_state_year:",onebyone_state_year)
            for y in onebyone_state_year:
                onebyone_state_year_json_path= onebyone_state+"/"+y
           # print("onebyone_state_year_json_path:", onebyone_state_year_json_path)
                onebyone_state_year_json_pos=os.listdir(onebyone_state_year_json_path)
            #print("onebyone_state_year_json_pos:", onebyone_state_year_json_pos)
                for onejson in onebyone_state_year_json_pos:
             # print("onejson:", onejson )
                    onebyone_state_year_json_pos_weneed=onebyone_state_year_json_path+"/"+onejson
                    files_open=open(onebyone_state_year_json_pos_weneed,"r")
                    files_final=json.load(files_open)
              #print(files_final)
                    for data in files_final["data"]["transactionData"]:
                        name=data["name"]
                        count=data['paymentInstruments'][0]["count"]
                        amount=data['paymentInstruments'][0]["amount"]
               
                
                        dic_state_yearbased["Transaction_amount"].append(amount)
                        dic_state_yearbased["Transaction_type"].append(name)
                        dic_state_yearbased["Transaction_count"].append(count)
                        dic_state_yearbased["year"].append(y)
                        dic_state_yearbased["quater"].append(int(onejson.strip(".json")))
                        dic_state_yearbased["state"].append(j)

# Converting the dataframe into csv file
df.to_csv('filename.csv',index=False)

# Step 3:
        Database insertion:
               engine=create_engine("mysql+pymysql://root:Risvana123@localhost:3306/phonepay_project")
               df.to_sql("sql_table_name",engine)
               
# Step 4:
          Data retrieval:
                  with engine.begin() as conn:
                        sql=text(f'select * from {table_name}')
                        df=pd.read_sql_query(sql,conn)
               
                
                  
                  
                  
