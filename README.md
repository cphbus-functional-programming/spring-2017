# General
This repository contains presentation PDFs and general information about the course.

## Study points
To attend the exam, you **need at least 17** (~80% of the total number) study points. The study points are listed here:
[Study points at Google Drive](https://docs.google.com/spreadsheets/d/1SAE1xZdOu8FmvlilVS5J2CjYqTFjEbWsyA9dQBhX5KE/edit?usp=sharing)

## Reading material
Haskell: [Learn you a Haskell for great good!](http://learnyouahaskell.com/chapters). Please read chapter 1-3

# Lisp tips and tricks
**Floating point overflow**: This error occurs when a number cannot contain enough
decimals. In the pi example we need 10.000 decimals, which is way more than a normal
float of 8 bytes can handle. Instead we need a biiiig number to be able to store 10.000 decimals.
Just like in Java, Lisp has different number types, and if you get a floating
point overflow, you need to coerce Lisp to use the very precise data type.
So when you call a function for the first time, you should append 'L0'
to your numbers. This will force Lisp to use the 'long-float datatype for
the remainder of your program.

Taking the pi exercise as an example, your call to your pi function could look like this:
   (myPi 1L0 (/ 1L0 (sqrt 2L0)) (/ 1L0 4L0) 1L0)
