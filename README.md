# MIAEC
Missing Imputation based on evidence chain
'''
Created on Jul 28, 2018

@author: Aleesha 
'''
import pandas as pd
import numpy as np
import types 
import openpyxl

from collections import Counter

from builtins import int


def MapClass(read_clients):
    
    row=read_clients.shape[0]
    col=read_clients.shape[1]
    MissingData=read_clients.fillna("NaN")
    print('Missing Data Filled with NaN is',MissingData)
    FinalData=read_clients.fillna("NaN")
    z=[]
    redOut=[]
    Shell=[]
    Petro=[]
    for c in range(0,col):
        i=1
        dv=0
        for r in range(1):
            dv=MissingData.iloc[r,c]
        if isinstance(dv, float) or isinstance(dv, int):
            z=[]
            a=0
            i=0  
            j=0
            mean=0
            for r in range(0,row-1):
                i+=1
                if MissingData.iloc[r,c]!='NaN':
                    a+= MissingData.iloc[r,c]
                    mean= a/i
                    mean=round(mean,2)
                else: 
                    z.append(mean)
                    MissingData.iloc[r,c]= mean
                    a+= mean
                    
            if  z==[]:
                print('')
            else:
                redOut=ReducerClass(z)
                
            for j in range(0,row):
                if FinalData.iloc[j,c]=='NaN':
                    FinalData.iloc[j,c]=redOut
        
        else :
            i=0
            d=[]
            m=[]
            for r in range(0,row):
                i+=1
                if MissingData.iloc[r,c]=='NaN':
                    MissingData.iloc[r,c] = Shell
                    m.append(Shell)
                    d.append(Shell)
                else :
                    d.append(MissingData.iloc[r,c])
                    Shell=categorical(d) 
                    print('The estimated value for this missing index is',Shell)
                    
            #print('The array input is', )
            print('the List of Estimated data is',m)
            Petro= categorical(m)
            for j in range(0,row):
                #print('petro is', Petro)
                if FinalData.iloc[j,c]=='NaN':
                    FinalData.iloc[j,c]=Petro
                    #print('Petro is', FinalData.iloc[j,c])
                #print('Output file is', FinalData)
            
 
    return FinalData 
               
    
def ReducerClass(Map_input):   
   
    print('The Reducer Input is',Map_input)
#     Map_input=list(set(Map_input))
    print('The Count',Counter(Map_input))
    
#     print(dict((i, Map_input.count(i)) for i in Map_input))
    reducerFinal=max(set(Map_input), key=Map_input.count)
    test=min(set(Map_input), key=Map_input.count)
    if(reducerFinal==test):
        reducerFinal=np.median(Map_input)
    print('Value with more confidence:',reducerFinal)
    
    
    return reducerFinal 

def categorical(Cat_input):
    i=0
    print('The Categorical Input is',Cat_input)
#     Map_input=list(set(Map_input))
    print('The Count',Counter(Cat_input))
#     print(dict((i, Map_input.count(i)) for i in Map_input))
    CatFinal=max(set(Cat_input), key=Cat_input.count)
    print('Value with more confidence:',CatFinal)
    print(i)
    return CatFinal 
    

def main():
    print("Hello World!")
    read_clients = pd.read_excel(r'F:\Datasets\Incomplete\lonosphere\Blend1.xlsx',header=None) 
    Missingdata= MapClass(read_clients)
    #print('type is', Missingdata.dtype)
    Missingdata.to_excel(r'F:\Datasets\latest\Ionosphere\Blend1_out.xlsx',header=None,index=False)
    
    
if __name__== "__main__":
    main()
