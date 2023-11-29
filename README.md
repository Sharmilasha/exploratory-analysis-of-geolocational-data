# exploratory-analysis-of-geolocational-data
  This project involves the use of K-Means Clustering to find the best accomodation fThis project involves the use of K-Means Clustering to find the best accommodation for students in Bangalore (or any other city of your choice) by classifying accommodation for incoming students on the basis of their preferences on amenities, budget and proximity to the location.
### code
```
import pandas as pd
data=pd.read_csv('food_coded.csv')
data
data.columns
column=['cook','eating_out','employment','ethnic_food', 'exercise','fruit_day','income','on_off_campus','pay_meal_out','sports','veggies_day']
d=data[column]
d
importseaborn as sns
sns.pairplot(d)
importnumpy as np
import pandas as pd
importmatplotlib.pyplot as plt
ax=d.boxplot(figsize=(16,6))
ax.set_xticklabels(ax.get_xticklabels(),rotation=30)
d.shape
s=d.dropna()
pip install folium
pip install minisom
## for data
importnumpy as np
import pandas as pd
## for plotting
importmatplotlib.pyplot as plt
importseaborn as sns
## for geospatial
import folium
importgeopy
## for machine learning
fromsklearn import preprocessing, cluster
importscipy
## for deep learning
importminisom
f=['cook','income']
X = s[f]
max_k = 10
## iterations
distortions = [] 
fori in range(1, max_k+1):
iflen(X) >= i:
model = cluster.KMeans(n_clusters=i, init='k-means++', max_iter=300, n_init=10, random_state=0)
model.fit(X)
distortions.append(model.inertia_)
## best k: the lowest derivative
k = [i*100 for i in np.diff(distortions,2)].index(min([i*100 for i
innp.diff(distortions,2)]))
## plot
fig, ax = plt.subplots()
ax.plot(range(1, len(distortions)+1), distortions)
ax.axvline(k, ls='--', color="red", label="k = "+str(k))
ax.set(title='The Elbow Method', xlabel='Number of clusters', 
ylabel="Distortion")
ax.legend()
ax.grid(True)
plt.show()
frompandas.io.json import json_normalize
import folium
fromgeopy.geocoders import Nominatim
import requests
CLIENT_ID="KTCJJ2YZ2143QHEZ2JAQS4FJIO5DLSDO0YN4YBXPMI5NKTE" 
# your Foursquare ID
CLIENT_SECRET="KNG2LO22BPLHN1E3OAHWLYQ5PQBN14XYZMEMAS0CPJEJKOTR"
 # your Foursquare Secret
VERSION = '20200316'
LIMIT = 10000
url='https://api.foursquare.com/v2/venues/explore?&client_id={}&client_secret={}&v={}&ll={},{}&radius={}&limit={}'.format(CLIENT_ID,CLIENT_SECRET, VERSION, 17.448372, 78.526957,30000,  LIMIT)
results = requests.get(url).json()
results
venues = results['response']['groups'][0]['items']
nearby_venues = json_normalize(venues)
nearby_venues
resta=[]
oth=[]
for	lat,long	in  zip(nearby_venues['venue.location.lat'],nearby_venues['venue.location.lng']):
url='https://api.foursquare.com/v2/venues/explore?&client_id={}&client_secret={}&v={}&ll={},{}&radius={}&limit={}'.format(CLIENT_ID,CLIENT_SECRET,  VERSION, lat,long,1000, 100)
res = requests.get(url).json()
venue = res['response']['groups'][0]['items']
nearby_venue = json_normalize(venue)
df=nearby_venue['venue.categories']

    g=[]
fori in range(0,df.size):
g.append(df[i][0]['icon']['prefix'].find('food'))
co=0
fori in g:
ifi>1:
co+=1
resta.append(co)
oth.append(len(g)-co)

nearby_venues['restaurant']=resta
nearby_venues['others']=oth
nearby_venues
lat=nearby_venues['venue.location.lat']
long=nearby_venues['venue.location.lng']
f=['venue.location.lat','venue.location.lng']
X = nearby_venues[f]
max_k = 10
## iterations
distortions = [] 
fori in range(1, max_k+1):
iflen(X) >= i:
model = cluster.KMeans(n_clusters=i, init='k-means++', max_iter=300, n_init=10, random_state=0)
model.fit(X)
distortions.append(model.inertia_)
## best k: the lowest derivative
k = [i*100 for i in np.diff(distortions,2)].index(min([i*100 for i
innp.diff(distortions,2)]))
## plot
fig, ax = plt.subplots()
ax.plot(range(1, len(distortions)+1), distortions)
ax.axvline(k, ls='--', color="red", label="k = "+str(k))
ax.set(title='The Elbow Method', xlabel='Number of clusters', 
ylabel="Distortion")
ax.legend()
ax.grid(True)
plt.show()
city = "Hyderabad"
## get location
locator = geopy.geocoders.Nominatim(user_agent="MyCoder")
location = locator.geocode(city)
print(location)
## keep latitude and longitude only
location = [location.latitude, location.longitude]
print("[lat, long]:", location)
nearby_venues.head()
nearby_venues.columns
n=nearby_venues.drop(['referralId', 'reasons.count', 'reasons.items', 'venue.id',
       'venue.name', 
       'venue.location.labeledLatLngs', 'venue.location.distance',
       'venue.location.cc', 
       'venue.categories', 'venue.photos.count', 'venue.photos.groups',
       'venue.location.crossStreet', 'venue.location.address','venue.location.city',
       'venue.location.state', 'venue.location.crossStreet',
       'venue.location.neighborhood',	'venue.venuePage.id',
       'venue.location.postalCode','venue.location.country'],axis=1)
n.columns
n
n=n.dropna()
n = n.rename(columns={'venue.location.lat': 'lat', 'venue.location.lng': 'long'})
n
n['venue.location.formattedAddress']
n
x, y = "lat", "long"
color = "restaurant"
size = "others"
popup = "venue.location.formattedAddress"
data = n.copy()

## create color column
lst_colors=["red","green","orange"]
lst_elements = sorted(list(n[color].unique()))

## create size column (scaled)
scaler = preprocessing.MinMaxScaler(feature_range=(3,15))
data["size"] = scaler.fit_transform(
data[size].values.reshape(-1,1)).reshape(-1)


## initialize the map with the starting location
map_ = folium.Map(location=location, tiles="cartodbpositron",
zoom_start=11)
## add points
data.apply(lambda row: folium.CircleMarker(
location=[row[x],row[y]],popup=row[popup],
radius=row["size"]).add_to(map_), axis=1)
## plot the map
map_
X = n[["lat","long"]]
max_k = 10
## iterations
distortions = [] 
fori in range(1, max_k+1):
iflen(X) >= i:
model = cluster.KMeans(n_clusters=i, init='k-means++', max_iter=300, n_init=10, random_state=0)
model.fit(X)
distortions.append(model.inertia_)
## best k: the lowest derivative
k = [i*100 for i in np.diff(distortions,2)].index(min([i*100 for i in np.diff(distortions,2)]))
## plot
fig, ax = plt.subplots()
ax.plot(range(1, len(distortions)+1), distortions)
ax.axvline(k, ls='--', color="red", label="k = "+str(k))
ax.set(title='The Elbow Method', xlabel='Number of clusters', 
ylabel="Distortion")
ax.legend()
ax.grid(True)
plt.show()
k = 6
model = cluster.KMeans(n_clusters=k, init='k-means++')
X = n[["lat","long"]]
## clustering
dtf_X = X.copy()
dtf_X["cluster"] = model.fit_predict(X)
## find real centroids
closest, distances = scipy.cluster.vq.vq(model.cluster_centers_, 
dtf_X.drop("cluster", axis=1).values)
dtf_X["centroids"] = 0
fori in closest:
dtf_X["centroids"].iloc[i] = 1
## add clustering info to the original dataset
n[["cluster","centroids"]] = dtf_X[["cluster","centroids"]]
n
fig, ax = plt.subplots()
sns.scatterplot(x="lat", y="long", data=n, 
palette=sns.color_palette("bright",k),
hue='cluster', size="centroids", size_order=[1,0],
legend="brief", ax=ax).set_title('Clustering (k='+str(k)+')')
th_centroids = model.cluster_centers_
ax.scatter(th_centroids[:,0], th_centroids[:,1], s=50, c='black', 
marker="x")
model = cluster.AffinityPropagation()
k = n["cluster"].nunique()
sns.scatterplot(x="lat", y="long", data=n, 
palette=sns.color_palette("bright",k),
hue='cluster', size="centroids", size_order=[1,0],
legend="brief").set_title('Clustering (k='+str(k)+')')
x, y = "lat", "long"
color = "cluster"
size = "restaurant"
popup = "venue.location.formattedAddress"
marker = "centroids"
data = n.copy()
## create color column
lst_elements = sorted(list(n[color].unique()))
lst_colors = ['#%06X' % np.random.randint(0, 0xFFFFFF) for i in 
range(len(lst_elements))]
data["color"] = data[color].apply(lambda x: 
lst_colors[lst_elements.index(x)])
## create size column (scaled)
scaler = preprocessing.MinMaxScaler(feature_range=(3,15))
data["size"] = scaler.fit_transform(
data[size].values.reshape(-1,1)).reshape(-1)
## initialize the map with the starting location
map_ = folium.Map(location=location, tiles="cartodbpositron",
zoom_start=11)
## add points
data.apply(lambda row: folium.CircleMarker(
location=[row[x],row[y]], 
color=row["color"], fill=True,popup=row[popup],
radius=row["size"]).add_to(map_), axis=1)
## add html legend
legend_html = """ """+color+""":"""
fori in lst_elements:
legend_html = legend_html+""" 
      """+str(i)+""""""
legend_html = legend_html+""""""
map_.get_root().html.add_child(folium.Element(legend_html))
lst_elements = sorted(list(n[marker].unique()))
data[data[marker]==1].apply(lambda row: 
folium.Marker(location=[row[x],row[y]], 
draggable=False,  popup=row[popup] ,       
icon=folium.Icon(color="black")).add_to(map_), axis=1)
## plot the map
map_
```
### Output:

![image](https://github.com/Sharmilasha/exploratory-analysis-of-geolocational-data/assets/94506182/9ee8587e-6e1c-4d5d-9d11-24b3e1b90c70)
