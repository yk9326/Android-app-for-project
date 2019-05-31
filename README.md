This app is part of a larger project (a pair of glasses), designed to work with a specific Raspberry Pi attached to the glasses and programmed as part of the project. I made the app, while others programmed the pi and designed the hardware of the glasses. 
People often rely on map applications to navigate a route. The intention of the project was to provide the people with an alternative option to navigating themselves through an area, that wouldn’t require looking down at their phone as much, and instead allow them to focus more on what is ahead of them. The glasses use image recognition to display what is ahead of the user. The initial goals for the glasses were to display road names on the roads within the view, and to display directions on the view. 
The app was written in java. The software tool used was Android Studio, the official integrated development environment for Google's Android operating system, designed specifically for Android development. 

![alt text](https://raw.githubusercontent.com/yk9326/Android-app-for-project/edit/master/to/block_diagram.png)

The app's main functions are to retrieve data such as map, origin, destination and route, and to send the data to the Pi via a Bluetooth connection. The data sent to the Pi are used by the Pi to display the view on the glasses. This app was made because the Pi does not always have internet connection to retrieve the data, and the app would better provide functions such as selecting destination. On startup, the app displays a list of nearby Bluetooth enabled devices. When user selects a device (intended to be the Pi), the app establishes a Bluetooth connection between the Android phone and the device (the Pi), the phone as the client and the pi as the server. Then, a Google Map is displayed, and the phone’s current location is automatically selected as the origin. The Google Map is brought on to the app using Google Maps SDK for Android. The user can select a destination by either entering the address or by clicking the destination on the map. Once a destination is provided, the route, the directions (e.g. turn right at College Ave), and images of the map (in largest dimension under the API’s limitation) along the route are retrieved from Google Maps API. The route is drawn on the map, and the images are tiled together to form a complete image that contains the entire route. The directions, coordinates (latitude, longitude) that make up the route when connected, and the completed image are converted to bytes and sent to the pi. In addition, coordinates of current location is sent to the pi when the phone is moved, and every 30 seconds. What were to be sent to the pi and their format were determined according to the pi’s needs.

To display the road names on the user’s view from the glasses, our first approach was to have the app retrieve a list of intersections (coordinate, road 1 name, road 2 name) from a website that returns the nearest intersection to a coordinate, using all the coordinates in a route, duplicates removed. However, we found that the intersections are not always on the route, because the closest intersection may not be part of the route. Our next approach was to have the app send to the pi an image of the map that contains the route, and to have the pi read the road names from the image. This approach was not successful, because the Pi was unable to read non-upright names on the map image. 
For glasses to display the view, we needed to know which way the user is facing. We did not have a certain way of knowing this, so we decided to assume that the user is facing the same direction as the phone. Therefore, the app sends the Pi the azimuth of the phone every 5 seconds. We found that at times the Pi was receiving multiple kinds of data in the same message line (e.g. receiving route and current location coordinates on the same message). To avoid this, I wrote the app to split data in chunks of 900 bytes, and send each one at a time. 
Some features added to the app in consideration for user-friendliness were refreshing the list of nearby Bluetooth devices when the list is swiped down in case of if the Pi is not discovered on the first time, being able to select the destination in more than one way, and periodically checking if GPS location and Bluetooth is still enabled on phone and their permissions allowed.
In writing the code, various sites on the web were referenced, including Google Maps Platform Documentation. 
