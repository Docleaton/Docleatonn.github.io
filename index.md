### CS-499-T3725 Computer Science Capstone Josh Leaton

# Self-Reflection
	
   I did not start my schooling at SNHU, I started it at UCCS (University of Colorado at Colorado Springs).  After having to
relocate I was able to transfer my credits to SNHU and continue my educational path. I was allowed to transfer my credits and
continue working on my degree. I have been using the skills learned at both schools to help me with personal projects. 
   
   This program has been useful in my growth as a programmer. It has helped me learn quite a bit about the process of creating a
functioning program. One of the things I really appreciated from the course was learning the agile method to create software.
Understanding this process has helped me stay organized while working on a project. 

# Portfolio Summary 
   The sections of code I selected for my portfolio are some of the better pieces of code I have worked on. They represent
examples of Software design and engineering, Algorithms and data structures, and databases. Each sections shows sections from each
of the three classes. Each section was chosen because I believe they are the best parts of each project I worked on.  

   The first section of code is from class CS-350-T3241 Emerging Sys Arch & Tech. The code file is Raspberrypitemphumid.py this is
a Python file. This code was developed for a Raspberry Pi device. The code is designed to read temperature and humidity from a
sensor and print out the results on a digital screen. This data is also stored in a separate file. This is a small project that
anyone could complete.

   The Second section of code is from the class CS-260-J1700 Data Structures and Algorithms. The code that I have selected is a
Binary Search tree, it is in C++. This project shows my work with data structures and algorithms. 

   The final section of code is from class CS-340-T4217 ClientServer Development. This is another Python file.  This project was
to create a database for an animal shelter. I chose the CRUD (Create, Read, Update, Delete) file as my example of databases. A
CRUD file is necessary in databases to do any editing in MongoDB. 

   


### Markdown - Raspberrypitemphumid.py


```markdown
import grovepi
import math
import time
from grove_rgb_lcd import *
import json #importing json libray


sensor = 4  #The port the sensor is in

blue = 0    

# Implement the JSON data storage and execute
path = '/home/pi/Desktop/School/Milestone 2/'
fileName = 'Milestonetwo'
ext = '.json'

filePathNameWExt = path + fileName + ext

def writeToJSONFile(path, fileName, data):
    
    with open(filePathNameWExt, 'w+') as fp:
        json.dump([temp,humidity],fp)
   

while True:
    try:
        
        [temp,humidity] = grovepi.dht(sensor,blue)
        
        temp = ((temp * 9) / 5.0) + 32 #Conversion from Celsius to Fahrenheit 
        
        print("temp = %.02f F humidity =%.02f%%"%(temp, humidity))
        t = str(temp)
        h = str(humidity)
        
        setRGB(127,0,64) #Set color on the back light to a purple.
        setText("Temp:" + t + "F      " + "Humidity :" + h + "%") # This displays the Temperature and Humidity
        
        time.sleep(60.0) # This updates the the reading every 60 seconds.
        writeToJSONFile(path, fileName, [temp,humidity])
    except IOError:
        print ("Error")


```
# Enhancement One
   This section of code I have selected is for a Raspberry Pi device that has sensors connected to it to measure the temperature and humidity. This code was made and works well, I chose this because it is a simple design and works. I have changes some of the comments that were more notes for myself and the professor to something more universal for anyone. The code is short so there was not a lot to be done with it other than cleaning it up a bit.


### Markdown - BinarySearchTree.cpp

``` Markdown 
#include <iostream>
#include <time.h>
#include "CSVparser.hpp"

using namespace std;

// forward declarations
double strToDouble(string str, char ch);

// define a structure to hold bid information
struct Bid {
    string bidId; // unique identifier
    string title;
    string fund;
    double amount;
    Bid() {
        amount = 0.0;
    }
};

// Internal structure for tree node
struct Node {
	Bid bid;
	Node* left;
	Node* right;

	//default constructor
	Node()  {
	left = nullptr;
	right = nullptr;

	}

	Node(Bid aBid) : Node() {
		this->bid = aBid;
	}
};


class BinarySearchTree {

private:
    Node* root;

    void addNode(Node* node, Bid bid);
    void inOrder(Node* node);
    Node* removeNode(Node* node, string bidId);

public:
    BinarySearchTree();
    virtual ~BinarySearchTree();
    void InOrder();
    void Insert(Bid bid);
    void Remove(string bidId);
    Bid Search(string bidId);
};

// Default constructor
 
BinarySearchTree::BinarySearchTree() {
    // initialize housekeeping variables
	root = nullptr;
}

// Destructor

BinarySearchTree::~BinarySearchTree() {

}

void BinarySearchTree::InOrder() {
}
// Insert a bid

void BinarySearchTree::Insert(Bid bid) {
    // Implement inserting a bid into the tree
	if (root == nullptr) {
		root = new Node(bid);
	}else  {
		this->addNode(root, bid);
	}
}

// Remove a bid

void BinarySearchTree::Remove(string bidId) {
    // Implement removing a bid from the tree
	this->removeNode(root, bidId);
}

// Search for a bid
 
Bid BinarySearchTree::Search(string bidId) {
    // Implement searching the tree for a bid

	//start searching from the root
	Node* current = root;

	//keeps looping downwards until it reaches bottom or bid is found
	while (current != nullptr)  {
		//if current node matches return it
		if (current->bid.bidId.compare(bidId) == 0)  {
			return current->bid;
		}
		//if bid is smaller than current then traverse left
		if (bidId.compare(current->bid.bidId) < 0 )  {
			current = current->left;
		}else  {
			current = current->right;
		}
	}

	Bid bid;
    return bid;
}

void BinarySearchTree::addNode(Node* node, Bid bid) {
    // Implement inserting a bid into the tree

	//if the node is larger than the bid add to the left subtree
	if(node->bid.bidId.compare(bid.bidId) > 0)  {
		if (node->left == nullptr)  {
			node->left = new Node(bid);
		}else  {
			this->addNode(node->left, bid);
		}
	}
	//add to the right subtree
	else  {
		if (node->right == nullptr)  {
			node->right = new Node(bid);
		}else  {
			this->addNode(node->right, bid);
		}

	}
}
void BinarySearchTree::inOrder(Node* node) {
}

Node* BinarySearchTree::removeNode(Node* node, string bidId)  {
	//if this node is null then return
	if (node == nullptr)  {
		return node;
	}

	//recurse down left subtree
	if (bidId.compare(node->bid.bidId) < 0)  {
		node->left = removeNode(node->left, bidId);
	}else if (bidId.compare(node->bid.bidId) > 0)  {
		node->right = removeNode(node->right, bidId);
	}else  {
		//no children so this is a leaf node
		if (node->left == nullptr && node->right == nullptr)  {
			delete node;
			node = nullptr;
		}
		//one child to the left
		else if (node->left != nullptr && node->right == nullptr)  {
		Node* temp = node;
		node = node->left;
		delete temp;
		}
		//one child to the right
				else if (node->right != nullptr && node->left == nullptr)  {
				Node* temp = node;
				node = node->right;
				delete temp;
		}
		//two children
				else  {
					Node* temp = node->right;
					while (temp->left != nullptr) {
							temp = temp-left;
			}
					node->bid = temp->bid;
					node->right = removeNode(node->right, temp->bid.bidId);
		}
	}
	return node;
}

 // Display the bid information to the console (std::out)
 
void displayBid(Bid bid) {
    cout << bid.bidId << ": " << bid.title << " | " << bid.amount << " | "
            << bid.fund << endl;
    return;
}

// Load a CSV file containing bids into a container

void loadBids(string csvPath, BinarySearchTree* bst) {
    cout << "Loading CSV file " << csvPath << endl;

    // initialize the CSV Parser using the given path
    csv::Parser file = csv::Parser(csvPath);

    // read and display header row - optional
    vector<string> header = file.getHeader();
    for (auto const& c : header) {
        cout << c << " | ";
    }
    cout << "" << endl;

    try {
        // loop to read rows of a CSV file
        for (unsigned int i = 0; i < file.rowCount(); i++) {

            // Create a data structure and add to the collection of bids
            Bid bid;
            bid.bidId = file[i][1];
            bid.title = file[i][0];
            bid.fund = file[i][8];
            bid.amount = strToDouble(file[i][4], '$');

            // push this bid to the end
            bst->Insert(bid);
        }
    } catch (csv::Error &e) {
        std::cerr << e.what() << std::endl;
    }
}

 // The one and only main() method
 
int main(int argc, char* argv[]) {

    // process command line arguments
    string csvPath, bidKey;
    switch (argc) {
    case 2:
        csvPath = argv[1];
        bidKey = "98109";
        break;
    case 3:
        csvPath = argv[1];
        bidKey = argv[2];
        break;
    default:
        csvPath = "eBid_Monthly_Sales_Dec_2016.csv";
        bidKey = "98109";
    }

    // Define a timer variable
    clock_t ticks;

    // Define a binary search tree to hold all bids
    BinarySearchTree* bst;

    Bid bid;

    int choice = 0;
    while (choice != 9) {
        cout << "Menu:" << endl;
        cout << "  1. Load Bids" << endl;
        cout << "  2. Display All Bids" << endl;
        cout << "  3. Find Bid" << endl;
        cout << "  4. Remove Bid" << endl;
        cout << "  9. Exit" << endl;
        cout << "Enter choice: ";
        cin >> choice;

        switch (choice) {

        case 1:
            bst = new BinarySearchTree();

            // Initialize a timer variable before loading bids
            ticks = clock();

            // Complete the method call to load the bids
            loadBids(csvPath, bst);

            //cout << bst->Size() << " bids read" << endl;

            // Calculate elapsed time and display result
            ticks = clock() - ticks; // current clock ticks minus starting clock ticks
            cout << "time: " << ticks << " clock ticks" << endl;
            cout << "time: " << ticks * 1.0 / CLOCKS_PER_SEC << " seconds" << endl;
            break;

        case 2:
            bst->InOrder();
            break;

        case 3:
            ticks = clock();

            bid = bst->Search(bidKey);

            ticks = clock() - ticks; // current clock ticks minus starting clock ticks

            if (!bid.bidId.empty()) {
                displayBid(bid);
            } else {
            	cout << "Bid Id " << bidKey << " not found." << endl;
            }

            cout << "time: " << ticks << " clock ticks" << endl;
            cout << "time: " << ticks * 1.0 / CLOCKS_PER_SEC << " seconds" << endl;

            break;

        case 4:
            bst->Remove(bidKey);
            break;
        }
    }

    cout << "Good bye." << endl;

	return 0;
}


```

# Enhancement Two
For this assignment I had to remove a few pieces of code one of which I did not do, it was in the original code from the school and was credited to a user on stack overflow. Because of this I don’t believe I should use that portion of code. I also removed to pieces of code that were commented out to help clean up the code. Because of this the code is much more compact and easier to read. I cleaned up some comments to make more sense to other readers.


### Markdown - AnimalShelter.py

```Markdown
from pymongo import MongoClient
class AnimalShelter(object):
    """ CRUD operations for Animal collection in MongoDB """

    def __init__(self, username=None, password=None):
        # Initializing the MongoClient. This helps to access the MongoDB databases and collections. 
        if username and password:
            self.client = MongoClient('mongodb://%s:%s@localhost:' % (username, password))
        else:
            self.client = MongoClient('mongodb://localhost:')
        self.database = self.client['project']

# Complete this create method to implement the C in CRUD.
    def create(self, data):
        if data is not None:
            insert = self.database.animals.insert(data)  # data should be dictionary 
            if insert!=0:
                return True
            else:
                return False           
        else:
            raise Exception("Nothing to save, because data parameter is empty")


    # Create method to implement the R in CRUD.
    def read(self,criteria=None):

        # criteria is not None then this find will return all rows which matches the criteria
        if criteria:
         # {'_id':False} just omits the unique ID of each row        
            
            data = self.database.animals.find(criteria,{"_id":False})
        else:
        #if there is no search criteria then all the rows will be return  
            data = self.database.animals.find( {} , {"_id":False})

        return data
#Create method to implement U in CRUD
    def update(self, data):
        if data is not None:
            update = self.database.animals.update(data)
            if update!=0:
                return True
            else:
                return False           
        else:
            raise Exception("Nothing to save, because data parameter is empty")
            
#create method to implement D in CRUD
    def delete(self, data):
        if project is not None:
            self.database.projects.remove(project.get_as_json())            
        else:
            raise Exception("Nothing to delete, because project parameter is None")

```
# Enhancement Three
For this assignment I chose the animalshelter.py code it is a basic CRUD file and a good example of setting up database features. It is a small bit of code but displays the creation of MongoDB CRUD, C is for Creation R is for Read U is for Update and D is for Delete. This Bit of code is important for working in a database. This bit of code did not need to be reworked at all, it is efficient and commented well.  So, because of this no changes were made.

