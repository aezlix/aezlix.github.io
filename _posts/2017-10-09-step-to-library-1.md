---
layout:     post
title:      Coding Objective-C Libraries in github
date:       2017-10-09 
summary:    This is an empty post to illustrate the pagination component with Pixyll.
categories: github
---

# Do you want to know how to code this?

![alt text](https://github.com/aezlix/aezlix.github.io/images/libs.PNG)

1. Make a file called `bit.h` and insert the code below into it.

```objective-c
typedef struct BIT {
  BIT(const BIT& b)
  : size(b.size), elems(0) {
    elems = new int[size];
    for(unsigned int i = 0; i < size; i++)
      elems[i] = b.elems[i];
  }

  BIT& operator = (const BIT& b) {
    delete[] elems;

    size = b.size;
    elems = new int[size];

    for(unsigned int i = 0; i < size; i++)
      elems[i] = b.elems[i];
    
    return *this;
  }

  BIT(unsigned int maxIdx) 
  : size(maxIdx + 1), elems(0) {
    elems = new int[size]();
  }

  ~BIT() {
    delete[] elems;
  }

  void add(unsigned int idx, int value) {
    idx++; // internally stored from index 1

    while(idx < size) {
      elems[idx] += value;
      idx += (idx & -idx);
    }
  }

  int readCumulative(unsigned int idx) {
    idx++; // internally stored from index 1

    int sum = 0;
    while(idx) {
      sum += elems[idx];
      idx = idx & (idx - 1);
    }

    return sum;
  }

  int readSingle(unsigned int idx) {
    idx++; // internally stored from index 1

    int sum = 0;
    if(idx) {
      sum = elems[idx];
      unsigned int common = idx - (idx & -idx);
      idx--;
      while(idx != common) {
        sum -= elems[idx];
        idx = idx & (idx - 1);
      }
    }
    return sum;
  }

  unsigned int size;
  int *elems;
} BIT;
```
