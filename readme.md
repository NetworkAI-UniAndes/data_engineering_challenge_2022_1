# Data Engineering Challenge NetworkAndesAI

Welcome to the data engineering challenge, your goal is to extract, transform and make data available to other users trough and API. 

The challenge aims to find the closest airport for a list of 100,000 users. To accomplish this you'll be able to retrieve a json with the position of the 100,000 users from the following [REST API](https://sccr8pgns0.execute-api.us-east-1.amazonaws.com/dev/locations). You'll need to retrieve each data point using the following syntax.

``` python 
requests.get('https://sccr8pgns0.execute-api.us-east-1.amazonaws.com/dev/locations/'+str(user_id)).json()["data"]
```
 Each data point contains an user id, longitude and latitude values.

To find the closest airports please use the list of registered airports in the [OurAirports](https://ourairports.com/) webpage. There you'll be able to find a dataset containing information on all airports on that site. That file is called **airports.csv**. This datafile contains the following schema: "id","ident","type","name","latitude_deg","longitude_deg","elevation_ft","continent","iso_country","iso_region","municipality","scheduled_service","gps_code","iata_code","local_code","home_link","wikipedia_link","keywords". 


<p align="center">
  <img src="https://www.collinsdictionary.com/images/full/airport_324754607.jpg" />
</p>

## Context 
------
Data engineering is the backbone of any tech company that relies on data. They're the one in charge that data is ready for consumption not only for the data scientists in the company but also for the final users. Creating a system that provides data reliably is not an easy task at all. It requires detailed knowledge of matematical transformations, set theory and software engineering skills.

Being a data engineer puts you in the center of both worlds: Backend engineering and data science. A data engineer is able to analyze big loads of data with high accuracy and efficiency. This challenge is aimed to challenge you to work with data that comes from difference sources and requires some special techniques to make it efficient to work with it. 

## Deliverable
------
Your deliverables are:
1. A csv file containing only the airports with wikipedia page. This data file must be called **airports_w_wiki.csv** and be uploaded in your github. 
2. A REST API that retrieves the closest airport for each user, and that allows the user to find the wikipedia page of it's closest airport by introducing their user id. 

As we're only interested into the airports with wikipedia pages, therefore we want you to create first a datafile containing only the airports with wikipedia page called **airports_w_wiki.csv**. This file must be uplodaded into your repository. 

After that we want you to create an API that returns the closest airport for each user. This API must be conected to a database containg 100,000 points with two fields: "user_id" and "airport_id". Then we must be able retrieve the data using the route **/nearest_airports/<user_id>**

Finally, this same API must also allow the user to query the wikipedia page of it's nearest aiport by introducing their user id as follows **/nearest_airports_wikipedia/<user_id>**. Then the api should return a json with the field "wikipedia_page" that contains the url of the airport wikipedia page.

## Evaluation
-----
We will evaluate your code accuraccy against the ground truth for the closest airports. We will do this using a **eval.py** script. This script will only need to change the **BASE_PATH** constant to work with your API, therefore be sure that your API is able to retrieve data with the following comands. 
``` python 
requests.get(BASE_PATH+'/nearest_airports/<user_id>').json()["data"]["airport_id"]
```
To retrieve the closest airport id 

``` python 
requests.get(BASE_PATH+'/nearest_airports_wikipedia/<user_id>').json()["data"]["wikipedia_page"]
```
To retrieve the wikipedia page for the closest airport.

Moreover, we will provide some test files so you can test the accuracy of your code. However, we will use the complete dataset in the evaluation part to prove that your code really works.

Your solution will weight 60% of the decision and your interview 40%. We will have the following weights for your calification matrix:

## Matrix
-----
| Challenge | Excellent  | Good | Satisfactory |  Insufficient  |
|-------------------------------|-------------|---------------|--------------|--------------------------|
|  Compute list of airports with wikipedia page  | **10 points** </br> </br> - All the airport ids with wikipedia page are in the script </br> - The code is clean and efficient  | **6 points** </br> </br> - All the airport ids with wikipedia page are in the script </br> </br> - the code is not efficient or easy to read |**4 points** </br> </br> - The list of airports is incomplete or include airports without wikipedia page  </br> - The code is not efficent|**0 points** </br> </br>  - The solution is wrong</br> </br>    |
|  API retrieving the closest airport for each user  | **50 points** </br> </br> - All the airport ids correspond to the closest airport </br> - The code is clean and efficient  | **40 points** </br> </br> - All the airport ids correspond to the closest airport </br> </br> - the code is not efficient or easy to read |**20 points** </br> </br> - The list of airports is incomplete or include wrong aiports ids  </br> - The code is not efficent|**0 points** </br> </br>  The solution is wrong</br> </br>  |
|  API retrieving the wikipedia page for the user's closest airport  | **40 points** </br> </br> - All the airport wikipedia pages correspond to the closest airport </br> </br>  - The code is clean and efficient  | **30 points** </br> </br> - All the airport ids correspond to the closest airport </br> </br> - the code is not efficient or easy to read |**15 points** </br> </br> - The list of airports is incomplete or include wrong aiports ids  </br> - The code is not efficent|**0 points** </br> </br> - The solution is wrong</br> </br>    |

Hints
------
- Remember that latitude and longitude are degrees in a sphere, therefore do not use a 2D plane to find the closest airport unless you have already projected the coordinates. 
- You can use the [Haversine formula](https://en.wikipedia.org/wiki/Haversine_formula) to find the distance between two points in a sphere.
- To get responses faster parallelize your code to focus on computing locations.
- Comparing all the points is not the most efficient way to find the closest neighbor.
- As the API have a limited reading capacity limit your parallel requests such that you perform around 60 reads per second.
- Program your script to handle errors in case you receive a bad http response. In this case you can speed your code to handle 344 reads per second.
- Use inner joins to retrieve your data.
