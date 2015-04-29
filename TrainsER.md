### Trains Model For Project 1
**John Ailor, Stephen Braccia**

#### High Level Description
This application is targeted at providing a simple reminder system for user's to sign-in and make "reservations" for their scheduale. These reservation's can be to help keep them on track, or keep them notified of pending trains. The primary focus of the application will be on the trains and train schedules. The application is able to track what type of coach is being used, and who is scheduled to operate the vehicle along the routes. 

#### Entities

| Entity | Rows | Description |
|-----|-----|--------|
| Conductor | *Id*, dob, name, location | The Train conductor is a member of personnel who is in charge of the entire a train including passengers, schedule, ect.|
| Train | *Id*, name | The train is the combination of engines and coaches that follows a schedule |
| Schedule | *Id*, Start, End | A schedule is the time that a train runs along a given map |
| Engine | *Id*, Company, Color, Pulling Power | Engine is the pulling force of the train |
| Coach | *Id*, Company, Build Year | Coach is the carrier cars that are being pulled by an engine and compose the rest of the train. 
| Station | *Id*, Location | A Station is a stop or location that a train stops on in order to load or unload passengers or cargo. 
| Engine Type | *Type*, Fuel Type | The engine type, much like a car model, is the manufacturers and production model of an engine | 
| Engineer | *Id*, dob, name, location | The Engineer is the personnel in charge of operating the train's engine. |
| Train Yard | *Id*, Location, Capacity | A train yard is a storage facility for trains and engines. Train yards also provide maintenance tasks. | 

| Relationship | Entities | Description |
|-----|-----|--------|
| Serviced By | ←Conductor, **Train** | 
| Follows | **←Train**, ←Schedule |
| Is(engine) | **←$Engine**, Engine Type |
| Leads | **←Train**, ←$Engine |
| Pulls | ←Coach, Train | 
| Maps | **Schedule**, **Station** | 
| Staffed By | ←Engineer, **Engine** |
| Stored At (1) | ←Engine, Train Yard | 
| Stored At (2) | ←Coach, Train Yard | 
| Links To | Station, Station [previous, next] | 




#### Data Acquisition 
Data will be acquired through a combination of scraping from Amtrak and Septa websites (APIs are available for some resources, but data is available via web interfaces). Schedule information is available via API, however train yard location, and station locations may only be available via web interfaces. General data on trains and coaches can be acquired through various online resources for train enthusiasts. Passenger/Conductor information will be input to the system by the user's, however without a strong user base, this information will be fabricated by the developers. 

#### User Interaction
User's will interact with the application by making reservation's for a train, and by fetching information regarding the schedule's in web interface programmed in Java. The user will be responsible for managing their own reservation, however all other information regarding their train will be populated for them automatically. 


