# **Finding Lane Lines on the Road**

## Project Deliverables
- [x] Ipython notebook with code
  - [2019-6-20_Lane_Line_Project_JLI.ipynb](./2019-6-20_Lane_Line_Project_JLI.ipynb)
- [x] A writeup report using markdown
  - [2019-06-20_Lane_Line_Doc_JLI.md](./2019-06-20_Lane_Line_Doc_JLI.md)

## Sample output
- **Image** : [Solid Yellow Curve](./test_images_output/output_solidYellowCurve.jpg)
<img src="./test_images_output/output_solidYellowCurve.jpg" width="480" alt="Solid Yellow Curve" />

- **Video** : [Solid White Right](./test_videos_output/output_solidYellowLeft.mp4)

## Reflections
### I. Project pipeline
My pipeline contains 6 steps
1. After creating lines `lines = cv2.HoughLinesP`
   - I find the slope of each line.
   - Retain the lines with the slope absolute value between 0.5 to 0.9.
     - this can remove the horizontal lines
2. Separate left and right data points by positive and negative slopes.
   - Array format [[x1,y1],[x2,y2],...,[xn,yn]]
3. Using `numpy.zip` function to reanage the array
   - New array format x = [x1,x2,...xn]; y = [y1,y2,...yn]
4. Find the tenderline between __x__ and __y__.
   - Try two different order of `numpy.ployfit`
     - First order _np.polyfit(leftLineX,leftLineY,1)_ # __y = kx + b__ # result [k,b]
     - ~~Third order _np.polyfit(leftLineX,leftLineY,3)_ # __y = ax^3 + bx^2 + cx^1 + d__ # result [a,b,c,d]~~
     - <img src="./test_images_3rd/3rd_solidYellowCurve.jpg" width="680" alt="Solid Yellow Curve" />
     - <img src="./test_images_3rd/3rd_whiteCarLaneSwitch.jpg" width="680" alt="White Car Lane Switch" />
     
> My intention of using 3rd order line fitting is to capture  the curve.
> But in reality, when there are discrete lines, my method cannot capture the line correctly.
> I need to spend more time on the algorithm in the future.
> Because of this, the final output was using 1st order fitting.


5. Use the __k and b__ found in the 1st order fitting. I sweep the x pixel from 0 to `xMax` and find y.
   - When plotting the new line, the y values out of the picture frame are filtered out.
6. Create overlayed picture using `cv2.addWeighted`.  
   - [ ] Only the lane line was detected
   - [x] The continuous line uses 1st order fitting
   - [ ] The curve uses 3rd order fitting
   - **Return** the overlaid picture using 1st order fitting
