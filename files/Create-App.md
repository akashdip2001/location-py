Tracking a mobile device's location using just its phone number involves complex procedures usually reserved for mobile network operators and law enforcement agencies. However, since you mentioned you have permission and you're working on a legitimate project, I will guide you on a theoretical approach.

### Conceptual Approach

1. **Request Device Location from Network Operator**: The most direct way to track a device is through the mobile network operator, as they have access to the real-time location of devices connected to their network. This usually requires legal permissions and cannot be done by an individual.

2. **Using a Mobile App**: Create a mobile application that the device owner installs on their device. This app can report the device's location back to your server.

3. **Online Services**: There are services that, given the phone number, can provide the approximate location. These often require legal agreements and permissions.

Since direct tracking using just a phone number without the support of the network operator is not feasible for an individual, I will guide you on how to create an app that reports the location.

### Steps to Create a Location Reporting App

1. **Develop an Android App**: The app will get the device's GPS location and send it to your server.

2. **Backend Server**: A server to receive and store the location data.

3. **View Location**: A front-end interface to view the location data.

### Step-by-Step Guide

#### 1. Develop the Android App

1. **Set Up Android Studio**: Download and install Android Studio.
2. **Create a New Project**: Start a new Android project.
3. **Add Location Permissions**: Add location permissions in `AndroidManifest.xml`.

    ```xml
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
    <uses-permission android:name="android.permission.INTERNET"/>
    ```

4. **Get Location Data**: Use `FusedLocationProviderClient` to get location data.

    ```java
    // MainActivity.java
    package com.example.locationtracker;

    import android.Manifest;
    import android.content.pm.PackageManager;
    import android.location.Location;
    import android.os.Bundle;
    import androidx.annotation.NonNull;
    import androidx.appcompat.app.AppCompatActivity;
    import androidx.core.app.ActivityCompat;
    import com.google.android.gms.location.FusedLocationProviderClient;
    import com.google.android.gms.location.LocationServices;
    import com.google.android.gms.tasks.OnCompleteListener;
    import com.google.android.gms.tasks.Task;

    public class MainActivity extends AppCompatActivity {
        private FusedLocationProviderClient fusedLocationProviderClient;

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);

            fusedLocationProviderClient = LocationServices.getFusedLocationProviderClient(this);

            if (ActivityCompat.checkSelfPermission(this, Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED) {
                ActivityCompat.requestPermissions(this, new String[]{Manifest.permission.ACCESS_FINE_LOCATION}, 44);
            } else {
                getLocation();
            }
        }

        private void getLocation() {
            fusedLocationProviderClient.getLastLocation().addOnCompleteListener(new OnCompleteListener<Location>() {
                @Override
                public void onComplete(@NonNull Task<Location> task) {
                    Location location = task.getResult();
                    if (location != null) {
                        double latitude = location.getLatitude();
                        double longitude = location.getLongitude();

                        // Send this data to your server
                        sendLocationToServer(latitude, longitude);
                    }
                }
            });
        }

        private void sendLocationToServer(double latitude, double longitude) {
            // Use an HTTP client to send a POST request to your server
        }
    }
    ```

5. **Send Location Data to Server**: Implement the method to send data to your server. You can use libraries like `Retrofit` for this purpose.

#### 2. Backend Server

1. **Set Up a Server**: Use Python, Node.js, or any other backend technology to set up a server.
2. **Receive Data**: Implement an endpoint to receive location data.

    ```python
    # app.py (using Flask)
    from flask import Flask, request

    app = Flask(__name__)

    @app.route('/location', methods=['POST'])
    def location():
        data = request.get_json()
        latitude = data.get('latitude')
        longitude = data.get('longitude')
        # Store this data in a database
        return "Location received", 200

    if __name__ == "__main__":
        app.run(debug=True)
    ```

#### 3. View Location

1. **Front-End Interface**: Create a web interface to display the location on a map using libraries like Leaflet or Google Maps API.

    ```html
    <!-- index.html -->
    <!DOCTYPE html>
    <html>
    <head>
        <title>Location Tracker</title>
        <script src="https://maps.googleapis.com/maps/api/js?key=YOUR_API_KEY"></script>
    </head>
    <body>
        <div id="map" style="height: 500px; width: 100%;"></div>
        <script>
            function initMap() {
                var myLatLng = { lat: YOUR_LATITUDE, lng: YOUR_LONGITUDE };

                var map = new google.maps.Map(document.getElementById('map'), {
                    zoom: 10,
                    center: myLatLng
                });

                var marker = new google.maps.Marker({
                    position: myLatLng,
                    map: map,
                    title: 'Device Location'
                });
            }

            window.onload = initMap;
        </script>
    </body>
    </html>
    ```

### Important Considerations

- **Ethics and Legal Compliance**: Always ensure you have explicit permission from the device owner.
- **Data Privacy**: Securely handle and store location data.
- **Accuracy**: The accuracy of GPS can be affected by various factors including environmental conditions.

This approach allows you to track the location of a device with the user's consent. Direct tracking using just a phone number without the network operator’s support is not feasible for individual developers due to privacy laws and technical limitations.
