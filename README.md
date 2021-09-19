# README

## Star's K Nearest Neighbors

### ✏️ Description

The Project uses a simple REPL to generate the k nearest neighbors of a star based on their euclidean distance based on
a given CSV file that contains star information and user's choice of star or coordinates.

### ⚙️ Design

This part of the project (involving star-knn) includes two main classes: Main.java and StarDistance.java. The main
classes includes the REPL that reads in INPUT data in the format of `stars data/stars/csv_file_name.csv`
and a query either in the format of `naive_neighbors k x y z` or `naive_neighbors k "star name"`. The file outputs the k
nearest neighbors of the star based on the name given or the (x,y,z) coordinate. The StarDistance class mainly handles
forming and calculating distances between stars. This data is stored in hashmaps when a new location/star is queried. It
has two main methods: `formCoordinate <x> <y> <z>` and `euclideanDistance <coordiate1> <coordinate2>`.

#### Assumptions:

1. There are no duplicate star names.
2. No star names contain a comma, but could have spaces, or other special characters.
3. When searching based on coordinates instead of a star name, even if the coordinate queried is the exact same as that
   of the star's, the returned results will contain the star with the same coordinate. If we are querying the star name,
   we will not include the star itself in the output. (see comparison between `k_nearest_to96GPSC_num.test`
   and `k_nearest_to_96GPSC.test`
   in additional system testing).

#### Implementations

Details of implementation please refer to Main.java and StarDistance.java file and comments. The general idea behind
implementation is to have 2 global hashmaps (hm1 and hm3) that stores kvpair of starName: Coordinate, and starName:
starID respectively, based on the assumption of not having duplicate star names; 1 array to store CSV information by
row; a counter that keeps track of the total number of stars; and a local hashmap (hm2) that pairs relative distance to
a list to star IDs, based on the assumption that there are equidistant stars to the star queried, which is generated
each time a star/coordinate is queried. When extracting the k-nearest neighbors, the local hashmap is sorted based on
distance (ascending) and starIDs are added to an array. If there are multiple stars at a given relative distance, the
star IDs are shuffled randomly and added to the array. The first k star IDs of the final array is printed if k is
smaller than total number of stars, otherwise the sorted star IDs of all stars are printed. Exceptions are handled at
each step of the query process.

#### Space & Time Analysis

The program uses O(n) space and O(n) time, where n is the number of stars in the CSV file. Methods such as
formCoordinate and euclideanDistance are O(1) time.

### 🐛 Errors & Bugs

The main errors of the program are a result of incorrect user INPUT. The program will log specific error message when
the user input an invalid file path, the csv format is wrong, doesn't follow the correct query format, or query a
non-existent star. One error is on testing the randomly picked stars that are equidistant, as the test-suite OUTPUTS are
manually picked, there could be error when testing on that. (More on this in the nest section). Though not achieved in
this iteration of pull request, a further improvement includes extracting the common code between the two
naive_neighbors methods and write them into a class to increase readability.

### 🧪 Tests

The program contains both JUnit test for methods in StarDistance class and system testing for the Main Class. JUnit
tests mostly test on the methods in StarDistance. In system testing, the test cases covers both finding k-nearest
neighbors and handling INPUT errors. Outside the given non-exhaustive system tests, I added test cases for both edge
cases and general functionalities.
`equi_distant_stars.test` tests the result of stars with the same relative distance to the star queried. There could be
failed test in this one, since the k neighbors are randomly picked. `k_nearest_to_96GPSC` and `k_nearest_to96GPSC_num`
tests the general cases of querying location and star name. The difference is that if the star coordinate is the same as
the query coordinate, then the star is included; but if the star is queried directly, it will not be included.
`more_k_than_stars.test` tests the cases where there's k is greater than the total number of stars, in which case we
output all the stars ranked by distance to the star/coordinate queried. `nearest_from_coordinate.test` tests general
functionality. `wrong_csv.test` tests the case where the csv file format is wrong (header information and input format
wrong).

### ✨ How to

To build use : `mvn package`

To run use:
`./run`

To conduct JUnit tests use: `mvn test`

To conduct system testing on stars use: `./cs32-test src/test/system/stars/*.test`

To start the server use:
`./run --gui [--port=<port>]`