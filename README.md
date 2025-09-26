#importing libraries needed
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sn
import os
#creating project folder
def projfold(maindr, projName, foldinx):
    folder = ['data', 'notebook', 'output','resources','visualization','transformations','model']
    os.chdir(maindr)
    os.makedirs(projName)
    newDir = maindr +'/'+projName
    os.chdir(newDir)

    for indx in foldinx:
        os.makedirs(folder[indx])
print(f"Folder structure created at: {project_path}")
print(f"Current working Directory is:{os.getcwd()}") 
projName = 'Market_Sales_Analysis'
maindr = r"C:\Users\DELL\Desktop\3MTT Data Science"
foldinx = [0,1,2,3,4,5,6]
projfold(maindr, projName, foldinx)
#function for loading file from the project folder
def fileloader(path, file, ext):
    if ext =='.csv':
        return pd.read_csv(path + '/' +file+ext)
    elif ext == '.xls' or'xlsl':
        return pd.read_xlsl(path +'/'+file+ext, axis)
#loading the dataset
path = r"C:\Users\DELL\Desktop\3MTT Data Science\Market_Sales_Analysis\data"
file = 'Dataset'
ext = '.csv'
df = fileloader(path,file,ext)
df
#picking the head
df.head(10)
#picking the tail
df.tail(10)
#setting option to display all rows in the dataset
pd.set_option('display.max.rows',200)
df
#checking the data information
df.info()
#checking for missing values
df.isna().sum()
#checking for duplicates in the dataset
duplicate = df[df.duplicated()]
duplicate
print("Measure of Dispersion in the data")
#checking for outliers in the dataset
df.plot(kind = 'box', subplots = True, layout= (3,3), sharex=False, sharey = False)
plt.gcf().set_size_inches(20,15)
path = r"C:\Users\DELL\Desktop\3MTT Data Science\Market_Sales_Analysis\visualization"
filename = "Box_plot"
savepath = (path +'/'+filename+'.png')
plt.savefig(savepath)
plt.show()
plt.close()
df.describe().T

'''Observations: The dataset is a sales data with 200 records and 7 features 1 float, 3 interger and 3 object
1. among the object data type the date must to be converted to date-time
2. two feature has missing values, Product with 10 and Total with 6 they must to be treated
3. The dataset contains no duplicates to be treated
4. The total feature contains outliers that needs to be treated but because of it representation we are leaving the ouliers as vilead observation'''
#Converting the date column to date-time data type
from datetime import datetime
df['Date'] = pd.to_datetime(df['Date'])
type(df['Date'])
#checkig to inspect the data types again
df.dtypes
#treating Missing Values in the dataset
df['Product'].value_counts()
#filling missing values
df['Product']=df['Product'].fillna(method ='ffill')
#filling total feature
df['Total'] = df['Total'].fillna(0)
#verifying no missing values
df.isna().sum()
#descibin the data
avgrP = df['Price'].mean() #the mean give us the average number of in total price of the products
avrgQ = df['Quantity'].mean()

minP = df['Price'].min() #the minimum price of the sels
minQ = df['Quantity'].min()

maxP = df['Price'].max() #the maximun price seld
maxQ = df['Quantity'].max()

most_sold = df['Product'].mode() # The ost repeated product i the product feature

Mini_Date = df['Date'].min() #checking at the minimum date in the date column
Mixi_Date = df['Date'].max() #checking at the miximum date in the date column

Total_Revenue = df['Total'].sum()

print('-------------------------------------------------------')
print('--------------------------------------------------------')
print(' ---------------------------------------------------------')
      
print('The average sells Price of this record is\n :====',minP)
print('the average Quantity sold in this record is\n :====',minQ)


print('The miximum Price of products sold this record is\n :====',maxP)
print('The miximum Quantity sold in this record is\n :====',maxQ)


print('The minimum Price of this record is\n :====',avgrP)
print('The minimum Quantity sold in this record is\n :====',avrgQ)

print('The first day this record was collected is\n :====',Mini_Date)
print('The last day this record was collected is\n :====',Mixi_Date)
print('The Most sold product in the sales data  is\n :====',most_sold)

print('Total Revenue for the generated in the period is \n:====', {Total_Revenue}) 



df
#Calculating number of product sold in the record
Product_count = df['Product'].value_counts()
Product_count
#plotting pairplot to see the relationship between the features
sn.pairplot(df)
plt.suptitle("Pairplot to view the relationship between the features")
path = r"C:\Users\DELL\Desktop\3MTT Data Science\Market_Sales_Analysis\visualization"
filename = 'pairplot for relationships'
savepath = (path + '/'+filename+'.png')
plt.savefig(savepath)
plt.show()
plt.close()

df['Date']=pd.to_datetime(df["Date"])
#Hourly = df.Date.index.Hour()
#Trend of total sales by product to identify trend of days, products sold and total price they sold at
plt.figure(figsize = (30,15))
sn.lineplot(
    data=df,
    x='Date', 
    y='Total',
    hue='Product',
    style='Product',  # Optional: Different line styles
    markers=True,     # Optional: Show markers
)
plt.title('Product-Wise Sales Trends')
plt.xlabel('Date')
path = r"C:\Users\DELL\Desktop\3MTT Data Science\Market_Sales_Analysis\visualization"
filename = 'Sales Trend'
savepath = (path + '/'+filename+'.png')
plt.savefig(savepath)
plt.show()
plt.close()
#grouping date, product and sale in weekly basis 
weekly_product_sales = df.groupby(
    [pd.Grouper(key='Date', freq='W-Mon'), 'Product']
)['Total'].sum().reset_index()
#Plotting weekly trnd to identify
plt.figure(figsize=(12, 6))
sn.lineplot(data =weekly_product_sales, x = 'Date', y='Total', style='Product', markers=True )
plt.title('Weekly Sales Trend (2024)')
plt.xlabel('Week Starting')
plt.ylabel('Revenue ($)')
plt.xticks(rotation=45)
path = r"C:\Users\DELL\Desktop\3MTT Data Science\Market_Sales_Analysis\visualization"
filename = 'Weekly Sales Trend'
savepath = (path + '/'+filename+'.png')
plt.savefig(savepath)
plt.show()
plt.close()
#grouping date, product and sale in weekly basis 
Monthly_product_sales = df.groupby(
    [pd.Grouper(key='Date', freq='ME'), 'Product']
)['Total'].sum().reset_index()
#Plotting Monthly trend 
plt.figure(figsize=(12, 6))
sn.lineplot(data =Monthly_product_sales, x = 'Date', y='Total', style='Product', markers=True )
plt.title('Monthly Sales Trend (2024)')
plt.xlabel('Month Starting')
plt.ylabel('Revenue ($)')
plt.xticks(rotation=45)
path = r"C:\Users\DELL\Desktop\3MTT Data Science\Market_Sales_Analysis\visualization"
filename = 'Monthly Sales Trend'
savepath = (path + '/'+filename+'.png')
plt.savefig(savepath)
plt.show()
plt.close()
plt.show()
df.columns
#Plotting count of product sold
plt.figure(figsize = (20,5))
Product_count.plot(kind='bar')
plt.title('Product Sold in a bar chart')
path = r"C:\Users\DELL\Desktop\3MTT Data Science\Market_Sales_Analysis\visualization"
filename = 'product bar chart'
savepath = (path + '/'+filename+'.png')
plt.savefig(savepath)
plt.show()
plt.close()
#plotting to see the Top 10 Customers by revanue
plt.figure(figsize = (20,5))

top_customers = df.groupby('CustomerID')['Total'].sum().sort_values(ascending=False).head(10)
top_customers.plot(kind='bar', title='Top 10 Customers by Revenue')
plt.xlabel("Total Amount sepend on products")
path = r"C:\Users\DELL\Desktop\3MTT Data Science\Market_Sales_Analysis\visualization"
filename = 'Top Ten Customers by Revenue Chart'
savepath = (path + '/'+filename+'.png')
plt.savefig(savepath)
plt.show()
plt.close()
plt.show()
# Top 10 products by revenue
plt.figure(figsize = (20,5))
top_products = df.groupby('Product')['Total'].sum().sort_values(ascending=False).head(10)
top_products.plot(kind='barh', title='Top 10 Products by Revenue')
path = r"C:\Users\DELL\Desktop\3MTT Data Science\Market_Sales_Analysis\visualization"
filename = 'Top 10 Products sold  by Revenue'
savepath = (path + '/'+filename+'.png')
plt.savefig(savepath)
plt.show()
plt.close()
#Looking at sold by product with their quantity
plt.figure(figsize = (20,20))
ax = sn.barplot(x ='Product', y = 'Price', data= df, hue ='Quantity', palette = 'pastel')
plt.title('sells by product with their quantity')
ax.set_xticklabels(ax.get_xticklabels(), rotation=75)
path = r"C:\Users\DELL\Desktop\3MTT Data Science\Market_Sales_Analysis\visualization"
filename = 'Product Sold By Quantity'
savepath = (path + '/'+filename+'.png')
plt.savefig(savepath)
plt.show()
plt.close()
#print the columns
df.columns
#screating data frame for correlation
sorted_cor = df[['OrderID',  'Quantity', 'Price',
       'Total']]
#Calculating the currealtion 
Corr_Matrix = sorted_cor.corr()
Corr_Matrix

#Correlation coefficiency
plt.figure(figsize=(20, 16))
sn.heatmap(Corr_Matrix, annot=True, cmap='coolwarm', vmin=-1, vmax=1)
plt.title('Correlation Heatmap')
path = r"C:\Users\DELL\Desktop\3MTT Data Science\Market_Sales_Analysis\visualization"
filename = 'Correlation Heatmap'
savepath = (path + '/'+filename+'.png')
plt.savefig(savepath)
plt.show()
plt.close()
Recommendations
#Recommendations:

'''1. Optimize Pricing Strategy
Issue: The average price is 
100 but price of products ranges from butpricesrangefrom 676 to $1,000, the hiher price point sold for laptop is $1000
to Increase revenue it is recommended that company should use a discount tier,
i.e put a boundle deal to laptop sold greater than 3 quantity to increase order size.

2. Increase average Order quantities
Issues: Average order quantity is 1 with a maximum of 3 quantities.
Recommendation is Company should use the Promotion starategy of buy 2 and get 10% off to push the quantity up to the closer
maximum of 3

3. Focus on Top perfomrning sold product i.e Laptops
Issues: Laptops are the top performing sold product but revenue may still be untapped
Recommendation are Upsell Accesseries e.g Buy Laptop and get the Laptop Bag and external warantie
Do targeted ads to attract more customers to see Lapto promo and discount to attract interest and buy.

4. Weekly Trend shows that Laptops highly sold at the midweek so adverts and promo should also target at the weekends to 
get more sales at the midweek and also attract customers to come and buy before midweek.

5. Investigate Low Quantity Anomalities
 The minimum quantity is sold 1.365 this has to be investigated if returns/refund are skwed and impliment return policy if needed'''





