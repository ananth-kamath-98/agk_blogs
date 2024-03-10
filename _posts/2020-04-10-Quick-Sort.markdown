---
layout: post
title:  "How I Learned Quick Sort!"
description: I always dreaded the idea of working with Quick Sort. Here, I show you how that changed.
date:   2020-04-10
categories: Algorithms Sorting
comments: False
---
<p>I was first introduced to Quick Sort in my Algorithm's class at college. All that I could understand from my professor was that the algorithm used to select an element as a pivot and then sort all the elements around it. I could never wrap my head around how it worked and how I could implement it in my projects. 
So naturally, I skipped all my exam questions that were related to it and chose to forget its existance. 
But now, I decided to face my fears and finally tackle the quick sort problem and find out what made it so "quick". Here's how I set about doing so and ended up being super comfortable with it. </p>
<br/>
<h3><b> What is Quick Sort? </b></h3>
Quick Sort is a Divide and Conquer algorithm. It begins by picking an element from the list, called <i>pivot</i> and then re-arranges the rest of the elements in the list around this pivot to finally give us a sorted array.
<h3><b> How Does It Work? </b></h3>
The first step is to choose an element as a pivot from the array of unsorted elements. The pivot can be selected in any of the following ways:<br>
<ul>
    <li>The first element of the array.</li>
    <li>The last element of the array. </li>
    <li>Any random element in the array. </li>
</ul>
After practicing for a while, I ended up choosing the first element as the pivot by habbit. I will stick to the same in this document.<br>
Once the pivot is picked, it is placed at its sorted position in the array and the elements that are smaller than it in value are placed to its left, and those that are larger than it in value are placed to its right. 
<br>
<br>The heart of the Quick Sort algorithm is the <i><b>patition</b></i> function. The partition function returns the sorted position of the pivot element. Once the pivot is placed in its sorted position, the array is divided (partitioned) at this index and the quick sort algorithm is recursively applied to both the partitions. In the end, we are left with the entirely sorted array. 
<br>
The pseudo code for the <b><i>partition</i></b> function is:

```
partition( low, high)
{
    pivot = A[ low ]
    i = low
    j = high

    while( i < j )
    {
        do
        {
            i++
        } while( A[i] <= pivot )
        
        do
        {
            j--;
        } while( A[j] > pivot )
        
        if ( i < j )
        {
            swap( A[i], A[j] )
        }
    }
    swap ( A[low], A[j] )
    return j;
}

```

A simplified explanation of this is:
<ul>
    <li><b>low</b> is the first index which is an element and <b>high</b> is the highest index which has an element.</li>
    <li><b>i</b> is assigned the value of <b>low</b>, <b>j</b> is assigned the value of <b>high</b> and <b>pivot</b> is assigned the value of <b>A[low]</b> </li>
    <li> Increment the value of <b>i</b> till we find a <b>A[i]</b> which is greater than <b>pivot</b>.</li>
    <li> Increment the value of <b>j</b> till we find a <b>A[j]</b> which is lesser than or equal to <b>pivot</b>.</li>
    <li> Once found, if <b>i</b><<b>j</b>, swap the values of <b>A[i]</b> and <b>A[j]</b>.</li>
    <li> Else if <b>i</b>><b>j</b>, swap <b>A[j]</b> and <b>A[pivot]</b>. Thus, <b>j</b> is the sorted position of the <b>pivot</b>.</li>
    <li> Finally, the algorithm returns the value of <b>j</b>, which is now pointing to the sorted position of the <b>pivot</b>.</li>
    <li> The list is partition at the <b>pivot</b> sorted position and the <b>QuickSort</b> function is called recursively. </li>
</ul>

The pseudo code for the <b>QuickSort()</b> function is:

```
QuickSort( low, high)
{
    if ( low < high )
    {
        pivot_position = partition ( low, high)

        QuickSort( low, pivot_position )
        QuickSort( pivot_position + 1, high)
    }
}
```

Now, the working of the algorithm made a lot of sense to me and suddenly, I wasn't so lost anymore.
The next step was to put my knowledge to test by working with an example. 
<br>
<h3><b> Let's Pair This With An Example! </b></h3>
<br>
Let me show you the example that I worked on along with a single run of the <b>partition</b> function.

```
A = [ 7, 2, 4, 1, 9, 6 ]

pivot = 0 and A[ pivot ] = 7
i = 0
j = 5

-> i is incremented till we find A[i] > A[pivot]
    i = 4
    A[i] = 9

-> j is decremented till we find A[j] <= A[pivot]
    j = 5
    A[j] = 6

-> Here, i < j and so we swap A[j] and A[i]
    A[i] = 6    i = 4
    A[j] = 9    j = 5

-> Increment i till A[i] > A[pivot]
    i = 5
    A[i] = 9

-> Decrement j till A[j] <= A[pivot]
    j = 4
    A[j] = 6

-> This time, i > j, so swap A[j] and A[pivot]
    A[j] = 7    j = 4
    A[pivot] = 6    pivot = 0

-> The function returns j and then QuickSort is again 
    called on the arrays to the left and to the right of j.

```

If that's too much of a head-scratcher, here's a visualisation of what I have done.

<div style="text-align: center">
    <img src="/images/quicksort.gif">
</div>
<br>
Thank you for making it to the end of the post. 
<br>
This is how I finally put in the time and effort to learn the workings of the Quick Sort algorithm which was initially a nightmare for me. I hope this blog helped you too. 
