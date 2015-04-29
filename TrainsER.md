### Trains Model For Project 1
**John Ailor, Stephen Braccia**

#### High Level Description
This application is targeted at providing a simple reminder system for user's to sign-in and make "reservations" for their scheduale. These reservation's can be to help keep them on track, or keep them notified of pending trains. The primary focus of the application will be on the trains and train schedules. The application is able to track what type of coach is being used, and who is scheduled to operate the vehicle along the routes. 

#### Entities

| Entity | Rows | Description |
|-----|-----|--------|
| Conductor | *Id*, dob, name, location | 
| Train | *Id*, name |
| Schedule | *Id*, Start, End | 
| Engine | *Id*, Company, Color, Pulling Power |
| Coach | *Id*, Company, Build Year |
| Station | *Id*, Location | 
| Engine Type | *Type*, Fuel Type | 
| Engineer | *Id*, dob, name, location | 
| Train Yard | *Id*, Location, Capacity | 

| Relationship | Entities | Description |
|-----|-----|--------|
| Serviced By | $\leftarrow$Conductor, **Train** |
| Follows | **$\leftarrow$Train**, $\leftarrow$Schedule |
| Is(engine) | **$\leftarrow$Engine**, Engine Type |
| Leads | **$\leftarrow$Train**, $\leftarrow$Engine |
| Pulls | $\leftarrow$Coach, Train | 
| Maps | **Schedule**, **Station** | 
| Staffed By | $\leftarrow$Engineer, **Engine** |
| Stored At (1) | $\leftarrow$Engine, Train Yard | 
| Stored At (2) | $\leftarrow$Coach, Train Yard | 
| Links To | Station, Station [previous, next] | 




#### Data Acquisition 
Data will be acquired through a combination of scraping from Amtrak and Septa websites (APIs are available for some resources, but data is available via web interfaces). Schedule information is available via API, however train yard location, and station locations may only be available via web interfaces. General data on trains and coaches can be acquired through various online resources for train enthusiasts. Passenger/Conductor information will be input to the system by the user's, however without a strong user base, this information will be fabricated by the developers. 

#### User Interaction
User's will interact with the application by making reservation's for a train, and by fetching information regarding the schedule's in web interface programmed in Java. The user will be responsible for managing their own reservation, however all other information regarding their train will be populated for them automatically. 


