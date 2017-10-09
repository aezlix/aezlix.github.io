---
layout:     post
title:      Coding Objective-C Libraries in Github, Part two
date:       2017-10-09 
summary:    This is the second part of the Objective-C Libraries series.
categories: jekyll pixyll
---

1. Make a file called `bit2D.h` and insert the code below into it.

```C++
typedef struct BIT2D {
  BIT2D(const BIT2D& b)
  : xsize(b.xsize), ysize(b.ysize), elems(0) {
    elems = new int*[xsize];
    for(unsigned int i = 0; i < xsize; i++) {
      elems[i] = new int[ysize]();
      for(unsigned int j = 0; j < ysize; j++)
        elems[i][j] = b.elems[i][j];
    }
  }

  BIT2D& operator = (const BIT2D& b) {
    for(unsigned int i = 0; i < xsize; i++)
      delete[] elems[i];
    delete[] elems;

    elems = new int*[xsize];
    for(unsigned int i = 0; i < xsize; i++) {
      elems[i] = new int[ysize]();
      for(unsigned int j = 0; j < ysize; j++)
        elems[i][j] = b.elems[i][j];
    }
    
    return *this;
  }

  BIT2D(unsigned int maxX, unsigned int maxY) 
  : xsize(maxX + 1), ysize(maxY + 1), elems(0) {
    elems = new int*[xsize];
    for(unsigned int i = 0; i < xsize; i++)
      elems[i] = new int[ysize]();
  }

  ~BIT2D() {
    for(unsigned int i = 0; i < xsize; i++)
      delete[] elems[i];
    delete[] elems;
  }

  void add(unsigned int x, unsigned int y, int value) {
    x++; 
    y++;

    while(x < xsize) {
      unsigned int y1 = y;
      while(y1 < ysize) {
        elems[x][y1] += value;
        y1 += (y1 & -y1);
      }
      x += (x & -x);
    }
  }

  int readCumulative(unsigned int x, unsigned int y) {
    x++; 
    y++;

    int sum = 0;
    while(x) {
      unsigned int y1 = y;
      while(y1) {
        sum += elems[x][y1];
        y1 = y1 & (y1 - 1);
      }
      x = x & (x - 1);
    }

    return sum;
  }

  int readSingle(unsigned int x, unsigned int y) {
    return readCumulative(x, y) - readCumulative(x - 1, y)
      - readCumulative(x, y - 1) + readCumulative(x - 1, y - 1);
  }

  unsigned int xsize, ysize;
  int **elems;
} BIT2D;
```
