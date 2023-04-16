---
title: "Creating a Dumb Suduko Solver, part 1"
summary: "Teaching myself OCR through OpenCV"
date: 2021-04-04T21:41:30-04:00

tags: [code, sudoku, python, computer vision]
categories: [code, python]
---

This was originally written in Janurary 2020.

Because this entire project is meant to teach myself OpenCV and OCR, I will be updating this as I go. Explaining how 
something works and my thought process behind it helps me better understand what is going on. It also might help me 
not forget what this is in the future.

## Initial Image Processing
Images should first be converted to gray-scale. A [Gaussian blur](https://en.wikipedia.org/wiki/Gaussian_blur) is a 
way to [smooth an image](https://docs.opencv.org/master/d4/d13/tutorial_py_filtering.html) and is added to reduce 
detail and image noise which helps with edge detection. 
[Adaptive thresholding](https://homepages.inf.ed.ac.uk/rbf/HIPR2/adpthrsh.htm) is added to further held find the 
edges of the sudoku grid. Adaptive threshold bases the changes on the pixels around it while binary threshold is a 
uniform change across the entire image. 

![Gray-scale, Gaussian, Adaptive threshold](/images/suduko/gray-gaus-adaptivethres.png)
(Left to right: Gray-scale, Gaussian blur, Adaptive threshold)

```python
   img = cv2.dilate(img, 
                    kernel=np.array([[0, 0, 0], [0, 1, 0], [0, 0, 0]], np.uint8), 
                    iterations=1)
```

Dilation is also in the code, but not used. I did not notice much of an improvement when adding it to the example image. 
I decided to leave it there just in case it is useful later on. Creating a larger `+`-shaped array causes the example 
images I used to look a bit strange. Some of the other solvers I looked at had toggles for dilation, which I might 
consider.

## Finding Contours
[Contours](https://docs.opencv.org/3.3.0/d4/d73/tutorial_py_contours_begin.html) help find the largest uninterrupted 
edges within the image. Because of the shape of the Sudoku grid, this is pretty straight forward. Below are two methods 
I found that accomplish this. It is too soon to tell which will end up being better, but it can be seen in the uncropped 
image, the left method picks up a secondary puzzle while the right only shows the largest contour found.

```python   
    img = cv2.cvtColor(img, cv2.COLOR_GRAY2RGB)
    img = cv2.drawContours(img, contours, -1, (0, 0, 255), 3)
```

![contours](/images/suduko/contours.png)

![largest contour block](/images/suduko/largest-block.png)

## Warping the contour
After determining the largest contour, it will grab the coordinates of the four corners and use that to warp the image 
to appear like it is looking straight on. 

```python
    matrix = cv2.getPerspectiveTransform(corner1, corner2)
    output = cv2.warpPerspective(original, matrix, (width,height))
```

This way it will be able to easily read the numbers in the puzzle. 
[images/sudoku-1](https://github.com/ottter/sudoku/blob/master/images/sudoku-1.jpg) picks up the bottom corner of the 
first row for some reason which I will have to figure out at some point.

![Warped image](/images/suduko/original-final.png)

## Creating the Cells
Sudoku is made up of 9x9 grid of cells, totaling 81. There seems to be a couple of ways to do this such as split the 
image in to 81 equal sized cells like below. From there I will use an OCR like Tesseract to identify the cell has 
contents, or pass each cell through a [dataset of font characters](http://www.ee.surrey.ac.uk/CVSSP/demos/chars74k/) 
to identify. If that doesn't consistently identify written numbers I can add MNIST as well, performance loss 
prohibiting. Future scope.
```python    
    cells = []
    side = img.shape[:1]
    side = side[0] / 9
    for i in range(9):
        for j in range(9):
            p1 = (i * side, j * side)        
            p2 = ((i + 1) * side, (j + 1) * side) 
            cells.append((p1, p2))
```

Find the entire project [here](https://github.com/ottter/sudoku).

