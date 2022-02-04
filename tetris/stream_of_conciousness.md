# Tetris Steam of Consciousness

## Position Comparison

Moving pieces, I saw one of the squares when to 0.499999 for X instead of 0.5. This caused the position.x == minX to fail, so need to create a comparator for floats that considers the fact that everything is 1 unit and floating errors will
be really small.  
Going to compare the two things so that it takes into account a level of tolerance. Like:

``` c#
    float tolerance = 0.15;
    if(Mathf.Abs(val1 - val2) <= tolerance) {
        // they are equal
    } 
```

would be how to compare them as actually being equal. Can return -1 if val1 < val2, 0 if val1 == val2, and 1 if val1 > val2.  This way I can compare pieces with grid and with other pieces to make sure it's a valid space.

## Show Preview of Where Piece Would Land

- Create a preview piece that's setup just like normal piece but with no fill on any of the squares. Do this for each shape.
- When a normal piece moves horizontally, or rotates, the preview piece should calculate the y-position that it would land on.
- On space bar pressed, set the actual piece y-pos to the preview piece y-pos. (Ok thinking about this..if you have a square that's lower than the pivot point, it wouldn't work exactly right.  Need to think about how to land the lowest square on the piece to where the preview piece should be)
- To calculate the y-pos, look at each y-position lower than each square in the actual piece and find the highest y-position that has a piece to land on.  If none of them do, then you will land on min-y.
- Optionally we could cut down on the number of checks by only checking squares on a different x or if it's a previously seen x, only check if the y is lower than the previous one you checked.  Actually...it would be the same y so you don't need to consider if it's higher..maybe just can ignore if we previously checked the x, and just update how we are looking once we figure out how to land the lowest point on the y and not the pivot point.
- Ok thinking about it, we want to find the difference between the lowest y-piece in each x value the piece takes up, and move the parent object down by that distance, which would land the piece correctly.
