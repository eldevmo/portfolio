---
title: "Intro to Python I"
date: 2019-06-02
tags: [Data Science]
header:
    image: "/images/projects.jpg"
excerpt: "Learning Basic Python syntax, writing simple Python functions, working with strings and dictionaries"
mathjax: "true"
---

## Data Science Certificate Project 1
### Goal
Learning Python syntax, writing simple Python functions, working with strings and dictionaries

**1. Count symbols**

In this question, you are given a string s which represents a DNA string. The string s consists of symbols 'A', 'C', 'G', and 'T'. An example of a length 21 DNA string is "ATGCTTCAGAAAGGTCTTACG."

Your task is to write a code which will count the number of times each of the symbols 'A', 'C', 'G', and 'T' occur in s. Your code should generate a list of 4 integers and print it out.

    s = 'AGCTTTTCATTCTGACTGCAACGGGCAATATGTCTCTGTGTGGATTAAAAAAAGAGTGTCTGATAGCAGC'

    count1 = 0
    count2 = 0
    count3 = 0
    count4 = 0

    for i in s:
        if i == 'A':
            count1 += 1
        elif i == 'C':
            count2 += 1
        elif i == 'G':
            count3 += 1
        elif i == 'T':
            count4 += 1
    print(count1, count2, count3, count4)

20 12 17 21

**2. Find a substring**

You are given a dictionary of the US states and their capitals. The keys in the dictionary are states and the values are capital names.

Write a code to return a list of all capitals that contain the name of a state in their name as a substring.

    capitals={
        'Alabama': 'Montgomery',
        'Alaska': 'Juneau',
        'Arizona':'Phoenix',
        'Arkansas':'Little Rock',
        'California': 'Sacramento',
        'Colorado':'Denver',
        'Connecticut':'Hartford',
        'Delaware':'Dover',
        'Florida': 'Tallahassee',
        'Georgia': 'Atlanta',
        'Hawaii': 'Honolulu',
        'Idaho': 'Boise',
        'Illinios': 'Springfield',
        'Indiana': 'Indianapolis',
        'Iowa': 'Des Monies',
        'Kansas': 'Topeka',
        'Kentucky': 'Frankfort',
        'Louisiana': 'Baton Rouge',
        'Maine': 'Augusta',
        'Maryland': 'Annapolis',
        'Massachusetts': 'Boston',
        'Michigan': 'Lansing',
        'Minnesota': 'St. Paul',
        'Mississippi': 'Jackson',
        'Missouri': 'Jefferson City',
        'Montana': 'Helena',
        'Nebraska': 'Lincoln',
        'Neveda': 'Carson City',
        'New Hampshire': 'Concord',
        'New Jersey': 'Trenton',
        'New Mexico': 'Santa Fe',
        'New York': 'Albany',
        'North Carolina': 'Raleigh',
        'North Dakota': 'Bismarck',
        'Ohio': 'Columbus',
        'Oklahoma': 'Oklahoma City',
        'Oregon': 'Salem',
        'Pennsylvania': 'Harrisburg',
        'Rhoda Island': 'Providence',
        'South Carolina': 'Columbia',
        'South Dakota': 'Pierre',
        'Tennessee': 'Nashville',
        'Texas': 'Austin',
        'Utah': 'Salt Lake City',
        'Vermont': 'Montpelier',
        'Virginia': 'Richmond',
        'Washington': 'Olympia',
        'West Virginia': 'Charleston',
        'Wisconsin': 'Madison',
        'Wyoming': 'Cheyenne'  
    }
    capitals

{'Alabama': 'Montgomery',
 'Alaska': 'Juneau',
 'Arizona': 'Phoenix',
 'Arkansas': 'Little Rock',
 'California': 'Sacramento',
 'Colorado': 'Denver',
 'Connecticut': 'Hartford',
 'Delaware': 'Dover',
 'Florida': 'Tallahassee',
 'Georgia': 'Atlanta',
 'Hawaii': 'Honolulu',
 'Idaho': 'Boise',
 'Illinios': 'Springfield',
 'Indiana': 'Indianapolis',
 'Iowa': 'Des Monies',
 'Kansas': 'Topeka',
 'Kentucky': 'Frankfort',
 'Louisiana': 'Baton Rouge',
 'Maine': 'Augusta',
 'Maryland': 'Annapolis',
 'Massachusetts': 'Boston',
 'Michigan': 'Lansing',
 'Minnesota': 'St. Paul',
 'Mississippi': 'Jackson',
 'Missouri': 'Jefferson City',
 'Montana': 'Helena',
 'Nebraska': 'Lincoln',
 'Neveda': 'Carson City',
 'New Hampshire': 'Concord',
 'New Jersey': 'Trenton',
 'New Mexico': 'Santa Fe',
 'New York': 'Albany',
 'North Carolina': 'Raleigh',
 'North Dakota': 'Bismarck',
 'Ohio': 'Columbus',
 'Oklahoma': 'Oklahoma City',
 'Oregon': 'Salem',
 'Pennsylvania': 'Harrisburg',
 'Rhoda Island': 'Providence',
 'South Carolina': 'Columbia',
 'South Dakota': 'Pierre',
 'Tennessee': 'Nashville',
 'Texas': 'Austin',
 'Utah': 'Salt Lake City',
 'Vermont': 'Montpelier',
 'Virginia': 'Richmond',
 'Washington': 'Olympia',
 'West Virginia': 'Charleston',
 'Wisconsin': 'Madison',
 'Wyoming': 'Cheyenne'}

    i = []

    for key in capitals.keys():
        if key in capitals[key]:
            i.append(capitals[key])
    print(i)

['Indianapolis', 'Oklahoma City']

**3. Is a data point within a rectangle?**

Write a function isIn() which returns boolean True if a point is within a rectangle specified by two sets of coordinates and boolean False if the point is outside the rectangle. The function should accept three parameters:

the first parameter is a set of coordinates which defines one of the corners of the rectangle,<br>
the second parameter is also a set of coordinates that defines the second corner,<br>
the third set of coordinates defines a single point which is being tested.

For example,

isIn((1,2), (3,4), (1.5, 3.2)) should return True,
isIn((4,3.5), (2,1), (3, 2)) should return True,
isIn((-1,0), (5,5), (6,0)) should return False,
isIn((4,1), (2,4), (2.5,4.5)) should return False.

    def isIn(firstCorner=(0,0), secondCorner=(0,0), point=(0,0)):
        # YOUR CODE HERE
        point_1, point_2 = point
        first_x, first_y = firstCorner
        second_x, second_y = secondCorner
        
        check1 = True
        check2 = True
        check3 = True
        check4 = True
        
        if abs(second_x) <= abs(point_1) <= abs(first_x):
            check1 = True
        
        if abs(first_x) <= abs(point_1) <= abs(second_x):
            check2 = True
        
        if abs(second_y) <= abs(point_2) <= abs(first_y):
            check3 = True
            
        if abs(first_y) <= abs(point_2) <= abs(second_y):
            check4 = True
        
        if check1 and check2 and check3 and check4 == True:
            return True
        else:
            return False

    print(isIn((3,2),(3,1),(5,2)))

False

    print(isIn((1,5),(2,9),(3,3.5)))

True

    print(isIn((0,15),(-3,1),(5,0)))

True

**4. Are all points within a rectangle?**

Modify your function from the previous question so it takes a list of points rather than a single point and returns boolean True only if all points in the list are in the rectangle.

For example,

allIn((0,0), (5,5), [(1,1), (0,0), (5,5)]) should return True
but allIn((0,0), (5,5), [(1,1), (0,0), (5,6)]) should return False
empty list of points allIn((0,0), (5,5), []) should return False

Use the same assumptions as above about the placement of the points and how rectangle is defined. Make sure that your function returns False for empty list of points (no values).

    def allIn(firstCorner=(0,0), secondCorner=(0,0), pointList=[]):
    
        first_x, first_y = firstCorner
        second_x, second_y = secondCorner
        
        check1 = True
        check2 = True
        check3 = True
        check4 = True
        check5 = True

        if pointList == []:
            return False
        
        else:
        
            for i in pointList:
                point_1, point_2 = i
            
                if abs(second_x) <= abs(point_1) <= abs(first_x):
                    check1 = True
        
                if abs(first_x) <= abs(point_1) <= abs(second_x):
                    check2 = True
        
                if abs(second_y) <= abs(point_2) <= abs(first_y):
                    check3 = True
            
                if abs(first_y) <= abs(point_2) <= abs(second_y):
                    check4 = True
                
                else:
                    check5 = False
                
        if check1 and check2 and check3 and check4 and check5 == True:
            return True
        else:
            return False
