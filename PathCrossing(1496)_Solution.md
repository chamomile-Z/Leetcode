## Approach 1
```python
def isPathCrossing(self, path: str) -> bool:
        # create an empty list
        q = []
        # beginning position
        x, y = 0, 0
        q.append((x,y))
        
        # adding each step position based on path
        for i in path:
            if i == 'N':
                x, y = x, y + 1
            elif i == 'S':
                x, y = x, y - 1
            elif i == 'E':
                x, y = x + 1, y
            elif i == 'W':
                x, y = x - 1, y
                
            # if the updated (x, y) is in q, then return True
            if (x, y) in q:
                return True
            # if not in q, added the new position in q 
            else:
                q.append((x, y))
        
        # after the iteration, if no (x, y) in q, then return false
        return(False)
```

