### Trains Model For Project 1
**John Ailor, Stephen Braccia**

#### High Level Description
This application is designed to track the operation and scheduling of Train entities, and is desgined for use by a Train Company (e.g. Amtrak, Septa, Patco). The primary focus of the application will be on the trains and train schedules. The application is able to track what type of coach is being used, and who is scheduled to operate the vehicle along the routes.

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
| Serviced By | ←Conductor, **Train** | Every Train must be serviced by at least 1 conductor, and each conductor must Service at most 1 train |
| Follows | **←Train**, ←Schedule | Every schedule must have 1 train, and every train needs at least 1 schedule | 
| Is(engine) | **←Engine**, Engine Type | Every Engine Type must have at least 1 engine, and every engine must be of only 1 engine type |
| Leads | **←Train**, ←Engine | Every Train needs to have at least 1 engine, but an engine can only be associated with a single train |
| Pulls | ←Coach, Train | Every Coach must be used on at most 1 Train | 
| Maps | **Schedule**, **Station** | Every Schedule needs at least 1 station, and every station must be utilized on at least 1 Schedule |
| Staffed By | ←Engineer, **Engine** | Every Engine needs at least 1 Engineer, and every Engineer needs to run at most 1 Engine |
| Stored At (1) | ←Engine, Train Yard | Every Engine needs to be stored at at most 1 train yard |
| Stored At (2) | ←Coach, Train Yard | Every Coach needs to be stored at most 1 Train Yard |  
| Links To | Station, Station [previous, next] | Stations are linked together, but a station does not need to have any links | 

*Key Constraints Participation Restraints are indicated by arrow: ←
*\*Participation Restraints  are indicated by **bold lettering**

#### Data Acquisition 
Data will be acquired through a combination of scraping from Amtrak and Septa websites (APIs are available for some resources, but data is available via web interfaces). Schedule information is available via API, however train yard location, and station locations may only be available via web interfaces. General data on trains and coaches can be acquired through various online resources for train enthusiasts. Engineer/Conductor information will be input to the system by the user's.

#### User Interaction
User's will interact with the application by querying for relevant information, and adjusting or adding new information via a web interface. 

### Table Creation

#### Entities 

	CREATE TABLE Conductors(
			id integer PRIMARY KEY 
			,dob
			,name
			,location
	)
	CREATE TABLE Train(
			id integer PRIMARY KEY 
			,name NOT NULL
	)
	CREATE TABLE Schedule(
			id integer PRIMARY KEY 
			,start NOT NULL
			,end NOT NULL
	)
	CREATE TABLE Engine(
			id integer PRIMARY KEY 
			,company NOT NULL
			,color NOT NULL
			,pulling_power NOT NULL
	)
	CREATE TABLE Coach(
			id integer PRIMARY KEY 
			,company NOT NULL
			,build_year NOT NULL
	)
	CREATE TABLE Station(
			id integer PRIMARY KEY 
			,location
	)
	CREATE TABLE Engine_type(
			id integer PRIMARY KEY 
			,type
			,fuel_type
	)
	CREATE TABLE Engineer(
			id integer PRIMARY KEY 
			,dob
			,name
			,location
	)
	CREATE TABLE Train_Yard(
			id integer PRIMARY KEY 
			,location
			,capacity
	)

#### Relationships
	CREATE TABLE Serviced_By(
		id integer PRIMARY KEY 
		,cId FOREIGN KEY REFERENCES Conductor (id) NOT NULL
		,tId FOREIGN KEY REFERENCES Train (id) UNIQUE
	)
	CREATE TABLE Follows(
		id integer PRIMARY KEY 
		,sId FOREIGN KEY REFERENCES Schedule (id) NOT NULL
		,tId FOREIGN KEY REFERENCES Train (id) UNIQUE NOT NULL
	)
	CREATE TABLE Is_engine(
		id integer PRIMARY KEY 
		,eId FOREIGN KEY REFERENCES Engine (id) NOT NULL
		,tId FOREIGN KEY REFERENCES Engine_Type (id) NOT NULL UNIQUE
	)
	CREATE TABLE Leads(
		id integer PRIMARY KEY 
		,eId FOREIGN KEY REFERENCES Engine (id) NOT NULL
		,tId FOREIGN KEY REFERENCES Train (id) UNIQUE
	)
	CREATE TABLE Pulls(
		id integer PRIMARY KEY 
		,cId FOREIGN KEY REFERENCES Coach (id)
		,tId FOREIGN KEY REFERENCES Train (id) UNIQUE
	)
	CREATE TABLE Maps(
		id integer PRIMARY KEY 
		,scId FOREIGN KEY REFERENCES Schedule (id) NOT NULL
		,stId FOREIGN KEY REFERENCES Station (id) NOT NULL
	)
	CREATE TABLE Staffed_By(
		id integer PRIMARY KEY 
		,erId FOREIGN KEY REFERENCES Engineer (id) UNIQUE NOT NULL
		,eId FOREIGN KEY REFERENCES Engine (id) NOT NULL
	)
	CREATE TABLE Stored_At_1(
		id integer PRIMARY KEY 
		,eId FOREIGN KEY REFERENCES Engine (id) UNIQUE
		,tId FOREIGN KEY REFERENCES Train_Yard (id) NOT NULL
	)
	CREATE TABLE Stored_At_2(
		id integer PRIMARY KEY 
		,cId FOREIGN KEY REFERENCES Coach (id) UNIQUE
		,tId FOREIGN KEY REFERENCES Train_Yard (id) NOT NULL
	)
	CREATE TABLE Links_To(
		id integer PRIMARY KEY 
		,previous FOREIGN KEY REFERENCES Station (id)
		,station FOREIGN KEY REFERENCES Station (id)
	)