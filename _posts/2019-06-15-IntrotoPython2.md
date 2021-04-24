---
title: "Intro to Python II"
date: 2019-06-15
tags: [Data Science]
header:
    image: "/images/projects.jpg"
excerpt: "Writing Python code that uses natural language processing techniques to figure out results."
mathjax: "true"
---

## Capstone Work from Codecademy
<img src="{{ site.url }}{{ site.baseurl }}/images/pythoncert.JPG" alt="python certificate">

### Goal
Jay Stacksby, billionaire and reality T.V. star, has been murdered at his isolated personal island before the season finale of his smash hit A Brand New Jay. One of the three contestants was responsible, but local authorities can’t figure out who. Write Python code that uses natural language processing techniques to figure out which contestant was the killer.



**Saving The Different Examples as Variables**

Save the murder note as a string in a variable called murder_note. Save Lily Trebuchet's introduction into lily_trebuchet_intro. Save Myrtle Beech's introduction into myrtle_beech_intro. Save Gregg T Fishy's introduction into gregg_t_fishy_intro.


    murder_note = "You may call me heartless, a killer, a monster, a murderer, but I\'m still NOTHING compared to the villian that Jay was. This whole ..."

    lily_trebuchet_intro = "Hi, I\'m Lily Trebuchet from East Egg, Long Island. I love cats, hiking, and curling up under a warm blanket with a book. So ..."

    myrtle_beech_intro = "Salutations. My name? Myrtle. Myrtle Beech. I am a woman of simple tastes. I enjoy reading, thinking, and doing my taxes. I entered ..."

    gregg_t_fishy_intro = "A good day to you all, I am Gregg T Fishy, of the Fishy Enterprise fortune. I am 37 years young. An adventurous spirit and ..."

**The First Indicator: Sentence Length**

Write a function get_average_sentence_length that takes some text as an argument. This function should return the average number of words in a sentence in the text.

    def get_average_sentence_length(text):
    
        number_words = len(text.split(' '))
    
        text = text.replace('?','.')
        text = text.replace('!','.')
    
        murder_note_splited = text.split('.')
    
        number_sentences = len(murder_note_splited) 
    
        return number_words / number_sentences

    print(get_average_sentence_length(murder_note))

20.6875


**Creating The Definition for Our Model**

Let's define a class called TextSample with a constructor. The constructor should take two arguments: text and author. text should be saved as self.raw_text. Call get_average_sentence_length with the raw text and save it to self.average_sentence_length. You should save the author of the text as self.author.

Additionally, define a string representation for the model. If you print a TextSample it should render:

The author's name<br>
The average sentence length

    class TextSample:
        def __init__(self,text,author):
            self.raw_text = text
            self.average_sentence_length = get_average_sentence_length(self.raw_text)
            self.author = author
            self.prepared_text = prepare_text(self.raw_text)
            self.word_count_frequency = build_frequency_table(self.prepared_text)
            self.ngram_frequency = build_frequency_table(ngram_creator(self.prepared_text))
        
        def __repr__(self):
            return self.author + ' ' + str(self.average_sentence_length)
        

**Creating our TextSample Instances**

Create a TextSample object for each of the samples of text that we have.

    murderer_sample for the murderer's note.
    lily_sample for Lily Trebuchet's note.
    myrtle_sample for Myrtle Beech's note.
    gregg_sample for Gregg T Fishy's note.

Print out each one after instantiating them.

    murderer_sample = TextSample(murder_note, 'murderer')
    lily_sample = TextSample(lily_trebuchet_intro, 'lily_trebuchet')
    myrtle_sample =  TextSample(myrtle_beech_intro, 'myrtle_beech')
    gregg_sample = TextSample(gregg_t_fishy_intro, 'gregg_t_fishy')

    print(murderer_sample)
    print(lily_sample)
    print(myrtle_sample)
    print(gregg_sample)

murderer 20.6875<br>
lily_trebuchet 15.05<br>
myrtle_beech 6.517241379310345<br>
gregg_t_fishy 12.625


**Cleaning Our Data**

Create a function called prepare_text that takes a single parameter text, makes the text entirely lowercase, removes all the punctuation and returns a list of the words in the text in order.

    def prepare_text(text):
        text = text.replace(',','')
        text = text.replace('?','')
        text = text.replace('.','')
        text = text.lower()
    
        split_text = text.split(' ')
    
        return split_text

    print(prepare_text("Where did you go, friend? We nearly saw each other."))

['where', 'did', 'you', 'go', 'friend', 'we', 'nearly', 'saw', 'each', 'other']
Update the constructor for TextSample to save the prepared text as self.prepared_text.


**Building A Frequency Table**

Create a function called build_frequency_table. It takes in a list called corpus and creates a dictionary called frequency_table. For every element in corpus the value frequency_table[element] should be equal to the number of times that element appears in corpus.

    def build_frequency_table(corpus):
        frequency_table = {}
        count = 0
        for i in corpus:

        
            if i in frequency_table.keys():
                count = frequency_table[i]
                count += 1
        
            else:
                count = 1
        
            frequency_table[i] = count
   
    
        return frequency_table

    print(build_frequency_table(['do', 'you', 'see', 'what', 'i', 'see']))
    print(build_frequency_table(['do', 'you', 'you', 'see', 'what', 'i', 'see', 'you','see']))

{'do': 1, 'you': 1, 'see': 2, 'what': 1, 'i': 1}
{'do': 1, 'you': 3, 'see': 3, 'what': 1, 'i': 1}


**The Second Indicator: Favorite Words**

Use build_frequency_table with the prepared text to create a frequency table that counts how frequently all the words in each text sample appears. Call these functions in the constructor for TextSample and assign the word frequency table to a value called self.word_count_frequency.

**The Third Indicator: N-Grams**

Create a function called ngram_creator that takes a parameter text_list, a treated in-order list of the words in a text sample. ngram_creator should return a list of all adjacent pairs of words, styled as strings with a space in the center.

    def ngram_creator(text_list):

        new_list = []
        
        for i in range(len(text_list)-1):
            new_list.append(text_list[i] +' ' +  text_list[i+1])
        
        return new_list

Use ngram_creator along with the prepared text to create a list of all the two-word ngrams in each TextSample. Use build_frequency_table to tabulate the frequency of each ngram. In the constructor for TextSample save this frequency table as self.ngram_frequency.

**Comparing Two Frequency Tables**

Write a function called frequency_comparison that takes two parameters, table1 and table2. It should define two local variables, appearances and mutual_appearances.

Iterate through table1's keys and check if table2 has the same key defined. If it is, compare the two values for the key -- the smaller value should get added to mutual_appearances and the larger should get added to appearances. If the key doesn't exist in table2 the value for the key in table1 should be added to appearances.

    def frequency_comparison(table1, table2):
        appearances = 0
        mutual_appearances = 0
    
        for key in table1.keys():
            if key in table2.keys() and table1[key] < table2[key]:
                mutual_appearances += (table1[key])
            
            elif key in table2.keys() and table1[key] > table2[key]:
                appearances += (table1[key])
            else:
                appearances += (table1[key])
            
        return mutual_appearances / appearances

    print(frequency_comparison(murderer_sample.word_count_frequency, lily_sample.word_count_frequency))
    print(frequency_comparison(murderer_sample.ngram_frequency, lily_sample.ngram_frequency))

0.16140350877192983
0.009174311926605505


**Comparing Average Sentence Length**

Write a function called percent_difference that returns the percent difference as calculated from the following formula:

$$\frac{|\ value1 - value2\ |}{\frac{value1 + value2}{2}}$$

In the numerator is the absolute value (use abs()) of the two values subtracted from each other. In the denominator is the average of the two values (value1 + value2 divided by two).

    def percent_difference(value1, value2):
    
        a = abs(value1 - value2)
        b = (value1 + value2) / 2
        c = a / b
    
        return c

    print(percent_difference(murderer_sample.average_sentence_length, lily_sample.average_sentence_length))
    
0.3154949282966072


**Scoring Similarity with All Three Indicators**

Calculate the percent difference of their average sentence length using percent_difference. Save that into a variable called sentence_length_difference. Since we want to find how similar the two passages are calculate the inverse of sentence_length_difference by using the formula abs(1 - sentence_length_difference). Save that into a variable called sentence_length_similarity.

Calculate the difference between their word usage using frequency_comparison on both TextSample's word_count_frequency attributes. Save that into a variable called word_count_similarity.

Calculate the difference between their two-word ngram using frequency_comparison on both TextSample's ngram_frequency attributes. Save that into a variable called ngram_similarity.

Add all three similarities together and divide by 3.

    def find_text_similarity(textSample1, textSample2):
    
        sentence_length_difference = percent_difference(textSample1.average_sentence_length, textSample2.average_sentence_length)
    
        sentence_length_similarity = abs(1 - sentence_length_difference)
    
        word_count_similarity = frequency_comparison(textSample1.word_count_frequency, textSample2.word_count_frequency)
    
        ngram_similarity = frequency_comparison(textSample1.ngram_frequency, textSample2.ngram_frequency)
    
        all_three = (sentence_length_similarity + word_count_similarity + ngram_similarity) / 3
    
        return all_three

**Rendering the Results**

Their name
Their similarity score to the murder letter

    print(lily_sample.author, find_text_similarity(murderer_sample, lily_sample))
    print(myrtle_sample.author, find_text_similarity(murderer_sample, myrtle_sample))
    print(gregg_sample.author, find_text_similarity(murderer_sample, gregg_sample))

lily_trebuchet 0.2850276308006427<br>
myrtle_beech 0.024300611988418153<br>
gregg_t_fishy 0.19571495939007189

**Who Dunnit?**

In the cell below, print the name of the person who killed Jay Stacksby.

    print(lily_sample.author + ' DID IT!!!')

lily_trebuchet DID IT!!!



