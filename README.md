import 'package:flutter/material.dart';
void main() {
runApp(MyApp());
}
Tells the Flutter framework to inflate the MyApp widget and make it the root of the widget tree displayed on the screen. class MyApp extends StatelessWidget {
const MyApp({super.key});
@override
Widget build(BuildContext context) {
return MaterialApp(
debugShowCheckedModeBanner: false,
title: "Navigation Activity",
theme: ThemeData(colorScheme: ColorScheme.light()),
home: HomeScreen(),
);
}
}
class HomeScreen extends StatelessWidget {
const HomeScreen({super.key});
// The data to be searched (const is needed because HomeScreen has a const constructor)
final List<String> coffees = const [
"Americano",
"Cappuccino",
"Espresso",
"Flat White",
"Latte",
"Macchiato",
"Mocha",
];
coffees list: A static list of string names for different coffee types. @override
Widget build(BuildContext context) {
return Scaffold(
appBar: AppBar(title: Text("Search")),
body: Column(
children: [
// Search bar (not working yet)
Padding(
padding: EdgeInsets.all(16),
child: TextField(
decoration: InputDecoration(
hintText: "Search coffee",
prefixIcon: Icon(Icons.search),
filled: true,
fillColor: Color(0xFFEEEEEE),

border: OutlineInputBorder(
borderRadius: BorderRadius.circular(30),
borderSide: BorderSide.none,
),
),
),

), TextField: Styled with a grey background (fillColor), a search icon prefix, and rounded borders, but missing an
onChanged function. // Shows every coffee for now

Expanded(
child: ListView.builder(
itemCount: coffees.length,
itemBuilder: (context, index) {
return ListTile(
leading: Icon(Icons.coffee),
title: Text(coffees[index]),
);
},
),
),
],
),
);
}
}
ListView.builder: Renders every single item in the coffees list regardless of what is typed into the search box.. Next we are going to make our search Work here through the use of:
1. Stateful Widget
2. Filtering Logic in the Build Method
3. Inetractive Search Bar and Dynamic Result
a) OnChanged
b) Ternary conditional Operator
// Changed to StatefulWidget so the screen can change when a tab is tapped
class HomeScreen extends StatefulWidget {
const HomeScreen({super.key});
@override
State<HomeScreen> createState() => _HomeScreenState();
}
class _HomeScreenState extends State<HomeScreen> {
int _selectedIndex = 0; // which bottom bar tab is active
 StatefulWidget: Enables the screen to manage state variables that change over time.  _searchText: A state variable that records the current text entered by the user (starts as an empty string ""). @override
Widget build(BuildContext context) {
// Keep only the coffees that contain the typed text
final results = coffees

.where((coffee) =>
coffee.toLowerCase().contains(_searchText.toLowerCase()))
.toList();

 .where(...): Filters the main coffees list. It checks if each coffee name contains the _searchText.  .toLowerCase(): Ensures the search is case-insensitive (so typing "latte" or "Latte" both match "Latte").  .toList(): Converts the filtered result back into a standard List<String>. return Scaffold(
appBar: AppBar(title: Text("Search")),
body: Column(
children: [
// Search bar
Padding(
padding: EdgeInsets.all(16),
child: TextField(
onChanged: (value) {
setState(() {
_searchText = value;
});
},
decoration: InputDecoration(
hintText: "Search coffee",
prefixIcon: Icon(Icons.search),
filled: true,
fillColor: Color(0xFFEEEEEE),
border: OutlineInputBorder(
borderRadius: BorderRadius.circular(30),
borderSide: BorderSide.none,
),
),
),
),
// Show the results, or a message when nothing matches
Expanded(
child: results.isEmpty
? Center(child: Text("No coffee found"))
: ListView.builder(
itemCount: results.length,
itemBuilder: (context, index) {
return ListTile(
leading: Icon(Icons.coffee),
title: Text(results[index]),
);
},
),
),
],
),
);
}
}

 onChanged: (value) { setState(() { _searchText = value; }); }: Captures every keystroke in the text field, updates _searchText, and calls setState to trigger an immediate screen rebuild.  results.isEmpty ? ... : ...: A ternary conditional operator:
 If no matches are found, it displays a center text message saying "No coffee found".


import 'package:flutter/material.dart';
void main() {
runApp(MyApp());
}
Tells the Flutter framework to inflate the MyApp widget and make it the root of the widget tree displayed on the screen. class MyApp extends StatelessWidget {
const MyApp({super.key});
@override
Widget build(BuildContext context) {
return MaterialApp(
debugShowCheckedModeBanner: false,
title: "Navigation Activity",
theme: ThemeData(colorScheme: ColorScheme.light()),
home: HomeScreen(),
);
}
}
class HomeScreen extends StatelessWidget {
const HomeScreen({super.key});
// A simple screen: a big icon with a title
Widget buildPlaceholderPage(IconData icon, String title) {
return Center(
child: Column(
mainAxisAlignment: MainAxisAlignment.center,
children: [
Icon(icon, size: 80),
SizedBox(height: 16),
Text(
title,
style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
),
],
),
);
}
@override
Widget build(BuildContext context) {
return Scaffold(
appBar: AppBar(title: Text("Navigation")),
// Always shows the Home page for now
body: buildPlaceholderPage(Icons.home, "Home"),
bottomNavigationBar: BottomNavigationBar(
type: BottomNavigationBarType.fixed,
currentIndex: 0,
selectedItemColor: Colors.orange,
unselectedItemColor: Colors.black,

onTap: (index) => print("Tapped index $index"),
items: const [
BottomNavigationBarItem(icon: Icon(Icons.home), label: "Home"),
BottomNavigationBarItem(icon: Icon(Icons.star), label: "Favorite"),
BottomNavigationBarItem(icon: Icon(Icons.settings), label: "Settings"),
BottomNavigationBarItem(icon: Icon(Icons.share), label: "Share"),
],
),
);
}
}
 buildPlaceholderPage(...): A helper method that creates a reusable layout template featuring a centered icon, vertical spacing (SizedBox), and bold text.  body: buildPlaceholderPage(...): Hardcodes the body to always show the Home page regardless of user
interaction.  onTap: (index) => print("Tapped index $index"): In this static version, tapping a tab only prints a message to
your debug console rather than changing the screen. Next we are going to make our navigation Work here through the use of:
1. Stateful Widget
2. Helper Methods
3. Changing the Scaffold
// Changed to StatefulWidget so the screen can change when a tab is tapped
class HomeScreen extends StatefulWidget {
const HomeScreen({super.key});
@override
State<HomeScreen> createState() => _HomeScreenState();
}
class _HomeScreenState extends State<HomeScreen> {
int _selectedIndex = 0; // which bottom bar tab is active
 StatefulWidget: Used when a widget needs to change its visual state based on user interaction (like tapping
a bottom navigation bar tab).  _selectedIndex: A state variable that tracks which tab number is currently active (starts at 0 for Home). Widget buildPlaceholderPage(IconData icon, String title) {
return Center(
child: Column(
mainAxisAlignment: MainAxisAlignment.center,
children: [
Icon(icon, size: 80),
SizedBox(height: 16),
Text(
title,
style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
),
],
),
);
}
// Pick which page to show based on the selected tab

Widget buildPage() {
if (_selectedIndex == 0) {
return buildPlaceholderPage(Icons.home, "Home");
} else if (_selectedIndex == 1) {
return buildPlaceholderPage(Icons.star, "Favorites");
} else if (_selectedIndex == 2) {
return buildPlaceholderPage(Icons.settings, "Settings");
} else {
return buildPlaceholderPage(Icons.share, "Share");
}

buildPage(): Evaluates the current value of _selectedIndex using conditional statements (if/else) and returns the
appropriate placeholder page (Home, Favorites, Settings, or Share) dynamically. @override
Widget build(BuildContext context) {
return Scaffold(
appBar: AppBar(title: Text("Navigation")),
body: buildPage(),
bottomNavigationBar: BottomNavigationBar(
type: BottomNavigationBarType.fixed,
currentIndex: _selectedIndex,
selectedItemColor: Colors.orange,
unselectedItemColor: Colors.black,
onTap: (index) {
setState(() {
_selectedIndex = index;
});
},
items: const [
BottomNavigationBarItem(icon: Icon(Icons.home), label: "Home"),
BottomNavigationBarItem(icon: Icon(Icons.star), label: "Favorite"),
BottomNavigationBarItem(icon: Icon(Icons.settings), label: "Settings"),
BottomNavigationBarItem(icon: Icon(Icons.share), label: "Share"),
],
),
);
}
}
 Scaffold: Provides the core high-level structure for the screen, providing slots for an appBar, body, and
bottomNavigationBar.  currentIndex: _selectedIndex: Highlights the correct tab icon in the bottom navigation bar depending on the
current state.  setState(() { _selectedIndex = index; }): Crucial Flutter mechanism. When a tab is tapped, setState
updates _selectedIndex and tells Flutter to rebuild the UI so buildPage() displays the newly selected tab's
content immediately.  BottomNavigationBarItem: Defines each individual tab's icon and text label displayed at the bottom of the


Name: ________________________________________ Course, Yr. & Sec: __________ Student Number: _______________________________ Date: _____________________
Instructions:
1. Copy this code to your previous Application.(Practical)
import 'package:flutter/material.dart';

void main(){
runApp(MyApp());
}
class MyApp extends StatelessWidget {
const MyApp({super.key});
@override
Widget build(BuildContext context) {
return MaterialApp(
debugShowCheckedModeBanner: false,
title: "My First App",
theme: ThemeData(colorScheme: ColorScheme.light()),
home: HomeScreen(),
);
}
}
class HomeScreen extends StatelessWidget {
const HomeScreen({super.key});
@override
Widget build(BuildContext context) {
return Scaffold(
// Let the background image go behind the app bar
extendBodyBehindAppBar: true,
appBar: AppBar(
backgroundColor: Colors.transparent,
elevation: 0,
foregroundColor: Colors.white,
title: Row(
children: [
ClipOval(
child: Image.asset(
"assets/images/logo.png",
width: 36,
height: 36,
fit: BoxFit.cover,
),
),
SizedBox(width: 12),

Republic of the Philippines
CAVITE STATE UNIVERSITY
Don Severino delas Alas Campus

Indang, Cavite

DCIT 26: Application Development and Emerging Technologies
Laboratory Exercise 4: StateFulWidget
Prepared by: John Aimiel C. Pena

Text("DCIT26", style: TextStyle(fontSize: 16)),
],
),
),
body: Container(
width: double.infinity,
height: double.infinity,
// Background image with a dark overlay
decoration: BoxDecoration(
image: DecorationImage(
image: AssetImage("assets/images/bakground.jpg"),
fit: BoxFit.cover,
colorFilter: ColorFilter.mode(
Colors.black.withOpacity(0.6),
BlendMode.darken,
),
),
),
child: SingleChildScrollView(
child: Column(
children: [
SizedBox(height: 100), // space for the app bar
// Search bar
Padding(
padding: EdgeInsets.symmetric(horizontal: 30),
child: TextField(
decoration: InputDecoration(
hintText: "Search here",
filled: true,
fillColor: Color(0xFFEEEEEE),
contentPadding:
EdgeInsets.symmetric(horizontal: 20, vertical: 14),
border: OutlineInputBorder(
borderRadius: BorderRadius.circular(30),
borderSide: BorderSide.none,
),
),
),
),
SizedBox(height: 50),
// ---------- Coffee 1 (text left, image right) ----------
Row(
children: [
Expanded(
child: Padding(
padding: EdgeInsets.only(left: 40, right: 16),
child: Column(

Republic of the Philippines
CAVITE STATE UNIVERSITY
Don Severino delas Alas Campus

Indang, Cavite

DCIT 26: Application Development and Emerging Technologies
Laboratory Exercise 4: StateFulWidget
Prepared by: John Aimiel C. Pena

crossAxisAlignment: CrossAxisAlignment.start,
children: [
Text(
"Coffee 1",
style: TextStyle(
fontSize: 16,
fontWeight: FontWeight.bold,
color: Colors.white,
),
),
SizedBox(height: 6),
Text(
"Lorem ipsum dolor sit amet, consectetur adipiscing elit.

Nullam quis varius odio.",

style: TextStyle(
fontSize: 13,
fontWeight: FontWeight.bold,
color: Colors.white,
),
),
SizedBox(height: 8),
ElevatedButton(
onPressed: () => print("Coffee 1 Buy Now"),
style: ElevatedButton.styleFrom(
backgroundColor: Color(0xFFE0E0E0),
foregroundColor: Colors.black,
shape: RoundedRectangleBorder(
borderRadius: BorderRadius.circular(8),
),
),
child: Text("Buy Now"),
),
],
),
),
),
ClipRRect(
borderRadius: BorderRadius.only(
topLeft: Radius.circular(20),
bottomLeft: Radius.circular(20),
),
child: Image.asset(
"assets/images/coffee.jpg",
width: 160,
height: 150,
fit: BoxFit.cover,
),
),
],

Republic of the Philippines
CAVITE STATE UNIVERSITY
Don Severino delas Alas Campus

Indang, Cavite

DCIT 26: Application Development and Emerging Technologies
Laboratory Exercise 4: StateFulWidget
Prepared by: John Aimiel C. Pena

),
SizedBox(height: 40),
// ---------- Coffee 2 (image left, text right) ----------
Row(
children: [
ClipRRect(
borderRadius: BorderRadius.only(
topRight: Radius.circular(20),
bottomRight: Radius.circular(20),
),
child: Image.asset(
"assets/images/coffee.jpg",
width: 160,
height: 150,
fit: BoxFit.cover,
),
),
Expanded(
child: Padding(
padding: EdgeInsets.symmetric(horizontal: 30),
child: Column(
crossAxisAlignment: CrossAxisAlignment.start,
children: [
Text(
"Coffee 2",
style: TextStyle(
fontSize: 16,
fontWeight: FontWeight.bold,
color: Colors.white,
),
),
SizedBox(height: 6),
Text(
"Lorem ipsum dolor sit amet, consectetur adipiscing elit.

Nullam quis varius odio.",

style: TextStyle(
fontSize: 13,
fontWeight: FontWeight.bold,
color: Colors.white,
),
),
SizedBox(height: 8),
ElevatedButton(
onPressed: () => print("Coffee 2 Buy Now"),
style: ElevatedButton.styleFrom(
backgroundColor: Color(0xFFE0E0E0),
foregroundColor: Colors.black,
shape: RoundedRectangleBorder(

Republic of the Philippines
CAVITE STATE UNIVERSITY
Don Severino delas Alas Campus

Indang, Cavite

DCIT 26: Application Development and Emerging Technologies
Laboratory Exercise 4: StateFulWidget
Prepared by: John Aimiel C. Pena

borderRadius: BorderRadius.circular(8),
),
),
child: Text("Buy Now"),
),
],
),
),
),
],
),
SizedBox(height: 40),
// ---------- Coffee 3 (text left, image right) ----------
Row(
children: [
Expanded(
child: Padding(
padding: EdgeInsets.only(left: 40, right: 16),
child: Column(
crossAxisAlignment: CrossAxisAlignment.start,
children: [
Text(
"Coffee 3",
style: TextStyle(
fontSize: 16,
fontWeight: FontWeight.bold,
color: Colors.white,
),
),
SizedBox(height: 6),
Text(
"Lorem ipsum dolor sit amet, consectetur adipiscing elit.

Nullam quis varius odio.",

style: TextStyle(
fontSize: 13,
fontWeight: FontWeight.bold,
color: Colors.white,
),
),
SizedBox(height: 8),
ElevatedButton(
onPressed: () => print("Coffee 3 Buy Now"),
style: ElevatedButton.styleFrom(
backgroundColor: Color(0xFFE0E0E0),
foregroundColor: Colors.black,
shape: RoundedRectangleBorder(
borderRadius: BorderRadius.circular(8),
),

Republic of the Philippines
CAVITE STATE UNIVERSITY
Don Severino delas Alas Campus

Indang, Cavite

DCIT 26: Application Development and Emerging Technologies
Laboratory Exercise 4: StateFulWidget
Prepared by: John Aimiel C. Pena
),
child: Text("Buy Now"),
),
],
),
),
),
ClipRRect(
borderRadius: BorderRadius.only(
topLeft: Radius.circular(20),
bottomLeft: Radius.circular(20),
),
child: Image.asset(
"assets/images/coffee.jpg",
width: 160,
height: 150,
fit: BoxFit.cover,
),
),
],
),
SizedBox(height: 40),
],
),
),
),
bottomNavigationBar: BottomNavigationBar(
type: BottomNavigationBarType.fixed,
backgroundColor: Colors.white,
currentIndex: 1,
selectedItemCoimport 'package:flutter/material.dart';

void main(){
runApp(MyApp());
}
class MyApp extends StatelessWidget {
const MyApp({super.key});
@override
Widget build(BuildContext context) {
return MaterialApp(
debugShowCheckedModeBanner: false,
title: "My First App",
theme: ThemeData(colorScheme: ColorScheme.light()),
home: HomeScreen(),
);
}

Republic of the Philippines
CAVITE STATE UNIVERSITY
Don Severino delas Alas Campus

Indang, Cavite

DCIT 26: Application Development and Emerging Technologies
Laboratory Exercise 4: StateFulWidget
Prepared by: John Aimiel C. Pena
}
class HomeScreen extends StatelessWidget {
const HomeScreen({super.key});
// The coffee data (const is needed because HomeScreen has a const constructor)
final List<Map<String, String>> coffees = const [
{
"name": "Coffee 1",
"description":
"Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nullam quis varius

odio.",
"image": "assets/images/coffee.jpg",
},
{
"name": "Coffee 2",
"description":
"Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nullam quis varius

odio.",
"image": "assets/images/coffee.jpg",
},
{
"name": "Coffee 3",
"description":
"Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nullam quis varius

odio.",
"image": "assets/images/coffee.jpg",
},
];
// One coffee row (image side depends on imageOnRight)
Widget buildCoffeeItem(Map<String, String> coffee, bool imageOnRight) {
Widget image = ClipRRect(
borderRadius: imageOnRight
? BorderRadius.only(
topLeft: Radius.circular(20),
bottomLeft: Radius.circular(20),
)
: BorderRadius.only(
topRight: Radius.circular(20),
bottomRight: Radius.circular(20),
),
child: Image.asset(
coffee["image"]!,
width: 160,
height: 150,
fit: BoxFit.cover,
),
);

Republic of the Philippines
CAVITE STATE UNIVERSITY
Don Severino delas Alas Campus

Indang, Cavite

DCIT 26: Application Development and Emerging Technologies
Laboratory Exercise 4: StateFulWidget
Prepared by: John Aimiel C. Pena
Widget text = Expanded(
child: Padding(
padding: EdgeInsets.only(
left: imageOnRight ? 40 : 30,
right: imageOnRight ? 16 : 30,
),
child: Column(
crossAxisAlignment: CrossAxisAlignment.start,
children: [
Text(
coffee["name"]!,
style: TextStyle(
fontSize: 16,
fontWeight: FontWeight.bold,
color: Colors.white,
),
),
SizedBox(height: 6),
Text(
coffee["description"]!,
style: TextStyle(
fontSize: 13,
fontWeight: FontWeight.bold,
color: Colors.white,
),
),
SizedBox(height: 8),
ElevatedButton(
onPressed: () => print("${coffee["name"]} Buy Now"),
style: ElevatedButton.styleFrom(
backgroundColor: Color(0xFFE0E0E0),
foregroundColor: Colors.black,
shape: RoundedRectangleBorder(
borderRadius: BorderRadius.circular(8),
),
),
child: Text("Buy Now"),
),
],
),
),
);
return Row(
children: imageOnRight ? [text, image] : [image, text],
);
}
// Search bar + coffee list

Republic of the Philippines
CAVITE STATE UNIVERSITY
Don Severino delas Alas Campus

Indang, Cavite

DCIT 26: Application Development and Emerging Technologies
Laboratory Exercise 4: StateFulWidget
Prepared by: John Aimiel C. Pena
Widget buildHomePage() {
return SingleChildScrollView(
child: Column(
children: [
SizedBox(height: 100), // space for the app bar
// Search bar (not working yet)
Padding(
padding: EdgeInsets.symmetric(horizontal: 30),
child: TextField(
decoration: InputDecoration(
hintText: "Search here",
filled: true,
fillColor: Color(0xFFEEEEEE),
contentPadding:
EdgeInsets.symmetric(horizontal: 20, vertical: 14),
border: OutlineInputBorder(
borderRadius: BorderRadius.circular(30),
borderSide: BorderSide.none,
),
),
),
),
SizedBox(height: 50),
// Show every coffee, alternating the image side
for (int i = 0; i < coffees.length; i++) ...[
buildCoffeeItem(coffees[i], i % 2 == 0),
SizedBox(height: 40),
],
],
),
);
}
@override
Widget build(BuildContext context) {
return Scaffold(
// Let the background image go behind the app bar
extendBodyBehindAppBar: true,
appBar: AppBar(
backgroundColor: Colors.transparent,
elevation: 0,
foregroundColor: Colors.white,
title: Row(
children: [
ClipOval(
child: Image.asset(
"assets/images/logo.png",

Republic of the Philippines
CAVITE STATE UNIVERSITY
Don Severino delas Alas Campus

Indang, Cavite

DCIT 26: Application Development and Emerging Technologies
Laboratory Exercise 4: StateFulWidget
Prepared by: John Aimiel C. Pena
width: 36,
height: 36,
fit: BoxFit.cover,
),
),
SizedBox(width: 12),
Text("DCIT26", style: TextStyle(fontSize: 16)),
],
),
),
body: Container(
width: double.infinity,
height: double.infinity,
// Background image with a dark overlay
decoration: BoxDecoration(
image: DecorationImage(
image: AssetImage("assets/images/bakground.jpg"),
fit: BoxFit.cover,
colorFilter: ColorFilter.mode(
Colors.black.withOpacity(0.6),
BlendMode.darken,
),
),
),
child: buildHomePage(),
),
bottomNavigationBar: BottomNavigationBar(
type: BottomNavigationBarType.fixed,
backgroundColor: Colors.white,
currentIndex: 0,
selectedItemColor: Colors.orange,
unselectedItemColor: Colors.black,
iconSize: 28,
showSelectedLabels: false,
showUnselectedLabels: false,
onTap: (index) => print("Tapped index $index"),
items: const [
BottomNavigationBarItem(icon: Icon(Icons.home), label: "Home"),
BottomNavigationBarItem(icon: Icon(Icons.star), label: "Favorite"),
BottomNavigationBarItem(icon: Icon(Icons.settings_outlined), label: "Settings"),
BottomNavigationBarItem(icon: Icon(Icons.share_outlined), label: "Share"),
],
),
);
}
}
lor: Colors.orange,
unselectedItemColor: Colors.black,
iconSize: 28,

Republic of the Philippines
CAVITE STATE UNIVERSITY
Don Severino delas Alas Campus

Indang, Cavite

DCIT 26: Application Development and Emerging Technologies
Laboratory Exercise 4: StateFulWidget
Prepared by: John Aimiel C. Pena
showSelectedLabels: false,
showUnselectedLabels: false,
onTap: (index) => print("Tapped index $index"),
items: const [
BottomNavigationBarItem(icon: Icon(Icons.home), label: "Home"),
BottomNavigationBarItem(icon: Icon(Icons.star), label: "Favorite"),
BottomNavigationBarItem(icon: Icon(Icons.settings_outlined), label: "Settings"),
BottomNavigationBarItem(icon: Icon(Icons.share_outlined), label: "Share"),
],
),
);
}
}
2. Make the Search bar and Navigation Bar Work

I WANT YOU TO FIX MY CODE. USE BASIC FLUTTER JUST LIKE THESE ONES. AND APPLY THESE THINGS IN MY CODE.
