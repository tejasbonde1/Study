# Study
#excel work using pandas
#importing the libraries that are needed
import os
import pandas as pd
from datetime import datetime
#Give parent path
path = ''
df_list1=[];
#Going to each folders and subfolders using os.walk function then 
#going through each files and check for the particular filename
def File_name(csv_name):
    for root, dirs, files in os.walk(path):
        for i in files:
            file_path=os.path.join(root,i)
#After getting the filename, taking the filname splitting it to get mr id and other info
            if file_path.endswith(csv_name):
                main_path=os.path.join(root,i)
                if file_path.endswith('ThicknessProfile.csv'):
                    splited_str=i.split('-');
                else:
                    splited_str=i.split('_')
            
                list1=[splited_str[0],splited_str[1]]
#extra work to get the date of visit for that splitting the path not req
            
                df=pd.read_csv(file_path,index_col=0)
#Using df.columns to differiante AO and PR info
                if file_path.endswith('ZonalMinimalStats.csv'):
                    df_col=list(df.columns)
                    main=[];
                    left=[];
                    for j in df_col:
                        spl=j.split('_')
                        if len(spl)==2:
                            main.append(spl[0])
                            left.append(spl[1])
                    df.columns=main
            
#updated_df=pd.DataFrame([main_heading])
                splited_path=main_path.split('\\');list2=splited_path[-2].split('-')
                list2[2]=datetime.strptime(list2[2],'%d%m%y')
                if file_path.endswith('ThicknessProfile.csv'):
                    main_heading={'Mr-Id':splited_str[0],'Eye':splited_str[1],'Date_of_Visit':list2[2]}
                else:
                    main_heading={'Mr-Id':splited_str[0],'Eye':splited_str[1],'Date_of_Visit':list2[2],'Info':left[0]}
                
                for col, value in main_heading.items():
                    df[col]=value
                final=pd.concat([df])
                df_list1.append(final)
        
             
            
# Concatenate DataFrames in df_list into a single DataFrame
    if csv_name=='ThicknessProfile.csv':
    
        combined_df= pd.concat(df_list1)
        final_csv=pd.DataFrame(combined_df)
   
#create the csv for the final data
        csv_path='E:\\Raghav\\combined_thickness.csv'
#Giving path where we want to save the csv
        final_csv.to_csv(csv_path)
    else:
        combined_df= pd.concat(df_list1)
        final_csv=pd.DataFrame(combined_df)
        print(final_csv.head(20))
        csv_path='E:\\Raghav\\Ao_PR_combined.csv'
    
#Giving path where we want to save the csv
        final_csv.to_csv(csv_path)
obj=File_name('ThicknessProfile.csv')
