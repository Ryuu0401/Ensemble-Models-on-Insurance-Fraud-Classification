1.3 Data Visualization

# let's take a look of the data relationship
# if we have a large dataset, then we should not use this due to too much time to cost
sns.pairplot(x)

![](images/Capture.JPG)

1.3.1. Univeriate Graphs: distribution, box-plot, histogram

fraud = df['fraud_reported'].value_counts()
label_fraud = fraud.index
size_fraud = fraud.values
colors = ['green', 'yellow']
trace = go.Pie(labels = label_fraud, values = size_fraud, marker = dict(colors = colors), name = 'Frauds')
layout = go.Layout(title = 'Distribution of Frauds')
fig = go.Figure(data = [trace], layout = layout)
py.iplot(fig)

# Retrieve all text variables from data
cat_cols = [c for i, c in enumerate(x.columns) if x.dtypes[i] in [np.object]]

# Retrieve all numerical variables from data
num_cols = [c for i, c in enumerate(x.columns) if x.dtypes[i] not in [np.object]]

import math
# let's firstly see the feature self plots (distribution, box-plot, histogram)

# plot the distribution for all the numeric features

plt.figure(figsize=(50,50))
k=1
for i in num_cols:
    plt.subplot(math.ceil(len(num_cols)/4),4,k)
    sns.distplot(x[i])
    k=k+1
plt.title('x[i]', fontsize = 10)
plt.show()

![](images/distribution.JPG)

# plot the distribution for all the numeric features

plt.figure(figsize=(50,50))
k=1
for i in num_cols:
    plt.subplot(math.ceil(len(num_cols)/4),4,k)
    sns.boxplot(x[i])
    k=k+1
plt.title('x[i]', fontsize = 10)
plt.show()

![](images/distributionpt2.JPG)

# text types histogram

plt.style.use('fivethirtyeight')
plt.figure(figsize=(50,50))
k=1
for i in cat_cols:
    plt.subplot(math.ceil(len(cat_cols)/4),4,k)
    sns.countplot(x[i], palette = 'spring')
    k=k+1
plt.title('x[i]', fontsize = 10)
plt.show()

![](images/histogram.JPG)

1.3.2. Multivariate Graphs: distribution, plots, histogram

cat_cols

![](images/catcols.JPG)


cat_keys=np.linspace(1, len(cat_cols), len(cat_cols)).astype(int)
cat_keys

![](images/catkeys.JPG)

dict_cat = {cat_keys[idx]:cat_cols[idx] for idx in range(len(cat_cols))} # for loop inside {}
dict_cat

![](images/dictcats.JPG)

# let's check the insured occupations

i=6
var=dict_cat[i]
occu = pd.crosstab(x[var], df['fraud_reported'])
occu.div(occu.sum(1).astype(float), axis = 0).plot(kind = 'bar', stacked = True, figsize = (15, 7))
plt.title('Fraud', fontsize = 20)
plt.xticks(rotation=45)
plt.legend()
plt.show()

![](images/insuredoccup.JPG)

i=5
j=12
var1=dict_cat[i]
var2=dict_cat[j]

cat_bar1 = pd.crosstab(df[var1], df[var2])
colors = plt.cm.Blues(np.linspace(0, 1, 5))
cat_bar1.div(cat_bar1.sum(1).astype(float), axis = 0).plot(kind = 'bar',
                                                           stacked = False,
                                                           figsize = (15, 7),
                                                           color = colors)
plt.title(var2, fontsize = 20)
plt.legend()
plt.show()

![](images/incidentsev.JPG)

i=5
j=12
var1=dict_cat[i]
var2=dict_cat[j]

cat_bar2 = pd.crosstab(df[var1], df[var2])
colors = plt.cm.inferno(np.linspace(0, 1, 5))
cat_bar2.div(cat_bar2.sum(1).astype(float), axis = 0).plot(kind = 'bar',
                                                           stacked = True,
                                                           figsize = (15, 7),
                                                           color = colors)

plt.title(var2, fontsize = 20)
plt.legend()
plt.show()

![](images/incidentsev2.JPG)

num_keys=np.linspace(1, len(num_cols), len(num_cols)).astype(int)
num_keys

![](images/numkeys.JPG)

dict_num = {num_keys[idx]:num_cols[idx] for idx in range(len(num_cols))} # for loop inside {}
dict_num

![](images/dictnum.JPG)

# numeric variable distribution by fraud

j=13
var_num=dict_num[j]

fig, axes = joypy.joyplot(df,
                         column = [var_num],
                         by = 'fraud_reported',
                         ylim = 'own',
                         figsize = (20, 10),
                         alpha = 0.5, 
                         legend = True)

plt.title(var_num, fontsize = 20)
plt.show()

![](images/totalclaim.JPG)

df.groupby('fraud_reported')['total_claim_amount'].describe()

![](images/groupby.JPG)

# Pairwise correlation
i=7
j=13
var1=dict_num[i]
var2=dict_num[j]

# plotting a correlation scatter plot
fig1 = px.scatter_matrix(df, dimensions=[var1, var2], color = "fraud_reported")
fig1.show()

# plotting a 3D scatter plot
fig2 = px.scatter(df, x = var1, y = var2, color = 'fraud_reported',
                marginal_x = 'rug', marginal_y = 'histogram')
fig2.show()

![](images/capitalgains.JPG)

# numeric variable distribution by categorical variable's level
i=7
j=13
var_cat=dict_cat[i]
var_num=dict_num[j]

fig, axes = joypy.joyplot(df,
                         column = [var_num],
                         by = var_cat,
                         ylim = 'own',
                         figsize = (20, 10),
                         alpha = 0.5, 
                         legend = True)

plt.title(var_num, fontsize = 20)
plt.show()

![](images/claimamt.JPG)

i=7
j=13
var_cat=dict_cat[i]
var_num=dict_num[j]

plt.style.use('fivethirtyeight')
plt.rcParams['figure.figsize'] = (15, 8)

sns.stripplot(df[var_cat], df[var_num], palette = 'bone')
plt.title(var_num, fontsize = 20)
plt.show()

![](images/hobbies.JPG)

i=7
j=13
var_cat=dict_cat[i]
var_num=dict_num[j]

plt.style.use('fivethirtyeight')
plt.rcParams['figure.figsize'] = (15, 8)

sns.boxenplot(df[var_cat], df[var_num], palette = 'pink')
plt.title(var_num, fontsize = 20)
plt.show()

![](images/newhobbies.JPG)

i=13
j=16
var_cat=dict_cat[i]
var_num=dict_num[j]

trace = go.Box(
          x = df[var_cat],
          y = df[var_num],
          opacity = 0.7,
          marker = dict(
                 color = 'rgb(215, 195, 5, 0.5)'
          )
)
data_boxplot = [trace]

layout = go.Layout(title = var_num)

fig = go.Figure(data = data_boxplot, layout = layout)
py.iplot(fig)

![](images/gobox.JPG)

i=1
j=3
k=9
l=13
m=14
n=15
var_cat1=dict_cat[i]
var_cat2=dict_cat[j]
var_cat3=dict_cat[k]
var_num1=dict_num[l]
var_num2=dict_num[m]
var_num3=dict_num[n]

var1=var_num1
var2=var_num2
var3=var_cat1

trace = go.Scatter3d(x = df[var1], y = df[var2], z = df[var3],
    mode = 'markers',  marker = dict(size = 10, color = df[var1]))

data_3d = [trace]

layout = go.Layout(
    title = ' ',
    margin=dict(
        l=0,
        r=0,
        b=0,
        t=0  
    ),
    scene = dict(
            xaxis = dict(title  = var_num1),
            yaxis = dict(title  = var_num2),
            zaxis = dict(title  = var_cat1)
        )
    
)
fig = go.Figure(data = data_3d , layout=layout)
py.iplot(fig)

![](images/3dgraph.JPG)

df=df.sort_values(by=['auto_year','months_as_customer'])

# dynamic graphs along time line
i=7
j=13
var1=dict_num[i]
var2=dict_num[j]

figure = bubbleplot(dataset = df, x_column = var1, y_column = var2, 
    bubble_column = 'fraud_reported', time_column = 'auto_year', size_column = 'months_as_customer', 
    color_column = 'fraud_reported', 
    x_title = var1, y_title = var2,
    x_logscale = False, scale_bubble = 3, height = 650)

py.iplot(figure, config={'scrollzoom': True})

![](images/dynamic.JPG)


1.4 Data Description

1.4.1. list variables' type and count

# list all the types of the features/variables in the dataset
# there are 3 types of features in total: int64, object and float64, in which the object is text
df.dtypes

![](images/dtypes.JPG)

# check the data information|
df.info(verbose=True)
# there are some missing values in the dataset

![](images/dinfo.JPG)

1.4.2. List the Frequency for each of the categorical variable

# list the frequency of each level for the categorical variables
x_categori=pd.DataFrame(df, columns=cat_cols)
for col in x_categori.columns:
    col_count=x_categori[col].value_counts()
    print('The Frequency for', col)
    print(col_count)
    print('Total:', x_categori[col].count())
    print('Missing Values:', len(x_categori[col])-x_categori[col].count())
    print('\n')
   
![](images/frequency.JPG)
   
# list the cross table between any two of the categorical variables
i=5
j=7
var1=cat_cols[i]
var2=cat_cols[j]
pd.crosstab(x[var1], x[var2])

![](images/crosstable.JPG)

1.4.3. Describe variales' Count, Mean, Standard Deviation, Minimum, Q1, Mediam, Q3, Max, MAD and Correlation

# describe all the numeric variabl
df.describe()

![](images/describe.JPG)

# compute the mean absolute deviation
x_numeric=pd.DataFrame(df, columns=num_cols)
x_numeric.mad().round(decimals = 3)

![](images/meanabsdev.JPG)

# compute the median absolute deviation
from statsmodels import robust
x_numeric=pd.DataFrame(df, columns=num_cols)
x_numeric.apply(robust.mad).round(decimals = 3)

![](images/medianabsdev.JPG)

# calculate the correlation on independent variables
x.corr()

![](images/correlation.JPG)


sns.heatmap(x.corr(), annot=True)

![](images/heatmap.JPG)

1.4.4. list the distribution of numeric variable at each level of selected categorical variable

i=5
j=15
cat_var=cat_cols[i]
num_var=num_cols[j]

print('distribution of', num_var, 'at', cat_var)
print('\n')
print (df.groupby(cat_var)[num_var].describe())

![](images/distvehicleclaim.JPG)


1.5 Data Preparation & Feature Analysis

1.5.1. Deal with Missing Values

# Change setting to display all columns and rows
pd.set_option("display.max_columns", None)
pd.set_option("display.max_rows", None)

# Display column missingness
missing=1 - x.count()/len(x.index)
missing.sort_values()

![](images/missingvalue.JPG)

print('\n')
# check the collision_type distribution
count_collision_type = x['collision_type'].value_counts()
print(count_collision_type)
print('\n')
# check the property_damage distribution
count_property_damage = x['property_damage'].value_counts()
print(count_property_damage)
print('\n')
# check the police_report_available distribution
count_police_report_available = x['police_report_available'].value_counts()
print(count_police_report_available)

![](images/collisiondist.JPG)

# we will replace the '?' by unknow since we do not know.
x['collision_type'].fillna('Unknown', inplace = True)

# It may be the case that there are no responses for property damage then we might take it as No property damage.
x['property_damage'].fillna('NO', inplace = True)

# again, if there are no responses fpr police report available then we might take it as No report available
x['police_report_available'].fillna('NO', inplace = True)

x.isnull().any().any()

# Display column missingness
x.isnull().sum()
# the result indicates there is no missing values

![](images/columnmissingness.JPG)

1.5.2 Drop some features

# now we need to consider dealing with the categorical variables
# we also drop auto_make since it is correlated to the auto_model, and auto_model is the subsegment of auto_make
# in addition, we drop the feature incident_date
# drop the location and the date also. they are not meaningful due to each location or date has almost only one incident.
# Thus no any date/location will cause more incident
x_keep=x.drop(['policy_bind_date','incident_date','incident_location', 'auto_model', 'total_claim_amount', 'age'], axis=1)

# Retrieve all text variables from data
cat_cols1 = [c for i, c in enumerate(x_keep.columns) if x_keep.dtypes[i] in [np.object]]

# Retrieve all numerical variables from data
num_cols1 = [c for i, c in enumerate(x_keep.columns) if x_keep.dtypes[i] not in [np.object]]

import math
# let's firstly see the feature self plots (distribution, box-plot, histogram)

cat_keys1=np.linspace(1, len(cat_cols1), len(cat_cols1)).astype(int)
dict_cat1 = {cat_keys1[idx]:cat_cols1[idx] for idx in range(len(cat_cols1))} # for loop inside {}
dict_cat1

![](images/catkeysdrop.JPG)

num_keys1=np.linspace(1, len(num_cols1), len(num_cols1)).astype(int)
dict_num1 = {num_keys1[idx]:num_cols1[idx] for idx in range(len(num_cols1))} # for loop inside {}
dict_num1 

![](images/numkeysdrop.JPG)

# install when use it first time
! pip install outlier_utils

import numpy as np
from outliers import smirnov_grubbs as grubbs

i=1

for i in range(len(num_cols1)):
    num_var=num_cols1[i]
    print('outlier detected for', num_var, 'the location is')
    grubbs.min_test_indices(x_keep[num_var], alpha=.05)
    grubbs.max_test_indices(x_keep[num_var], alpha=.05)
    print('outlier detected for ', num_var, ' the values is')
    grubbs.min_test_outliers(x_keep[num_var], alpha=.05)
    grubbs.max_test_outliers(x_keep[num_var], alpha=.05)
    print('\n')
    i=i+1
    
# the result indicates no outliers

![](images/outliersdetect.JPG)

1.5.4. Create Dummy Variables

# Encode categorical features into dataframe
x_dummy = pd.get_dummies(x_keep, columns = cat_cols1)
x_dummy.head(5)

![](images/dummyvar.JPG)

x_dummy.shape

x_check=x_dummy.loc[:, (x_dummy != 0).any(axis=0)]
x_check.shape

1.5.5. Standardize the features

# standardize the features
sc = StandardScaler()
x_scaled = sc.fit_transform(x_dummy)
x_scaled_df = pd.DataFrame(x_scaled, columns = x_dummy.columns)
x_scaled_df.head(10)

![](images/standardize.JPG)

x_scaled_df.describe()

![](images/standescribe.JPG)

1.5.6. Test Multicollinearity and Remove the High Correlated Feature

from statsmodels.stats.outliers_influence import variance_inflation_factor

vif_data = pd.DataFrame()
vif_data["feature"] = x_scaled_df.columns
  
# calculating VIF for each feature
vif_data["VIF"] = [variance_inflation_factor(x_scaled_df.values, i)
                          for i in range(len(x_scaled_df.columns))]   
                          
vif_data.sort_values("VIF", ascending=False)

![](images/vif.JPG)

x_categori=pd.DataFrame(x_keep, columns=cat_cols1)
for col in x_categori.columns:
    col_count=x_categori[col].value_counts()
    print('The Frequency for', col)
    print(col_count)
    print('Total:', x_categori[col].count())
    print('Missing Values:', len(x_categori[col])-x_categori[col].count())
    print('\n')
    
![](images/policefrequency.JPG)
    
pd.crosstab(df['fraud_reported'], df['incident_city'])

![](images/fraudcross.JPG)

x_final=x_scaled_df.drop(['insured_hobbies_camping', 'incident_type_Vehicle Theft', 
'incident_severity_Trivial Damage', 'authorities_contacted_None', 'insured_occupation_adm-clerical', 'insured_education_level_Masters', 
'insured_relationship_husband', 'collision_type_Unknown', 'incident_city_Northbrook', 'incident_state_PA', 'auto_make_Jeep' ], axis=1)
# x_final=x_scaled_df

# test vif for features matrix
from statsmodels.stats.outliers_influence import variance_inflation_factor

vif_data = pd.DataFrame()
vif_data["feature"] = x_final.columns
  
# calculating VIF for each feature
vif_data["VIF"] = [variance_inflation_factor(x_final.values, i)
                          for i in range(len(x_final.columns))]   
vif_data.sort_values("VIF", ascending=False)

![](images/postvif.JPG)

x_final.shape

x_final.head(10)

![](images/finalhead.JPG)

1.5.6 Resampling the Data

#split the dastaset into train, validation and test with the ratio 0.7, 0.2 and 0.1
X_train, X_test, y_train, y_test = train_test_split(x_final, y, test_size = 0.1, random_state=321) #Predictor and target variables
X_train, X_val, y_train, y_val = train_test_split(X_train, y_train, test_size=0.22222222222222224, random_state=321) 

