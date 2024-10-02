#### Object recognition
Different types of recognition
- object identification
- object classification

##### Which level is right for Object Classes?
- Basic-Level Categories

###### Challenges
- multi-view: different view points
- multi-class: different types of the same object (different car models)
- varying illumination
- ecc

### Filtering basics
- Linear filtering
	- Gaussian filtering
- Multi scale image representation
	- gaussian pyramid
- edge detection
	- recognition using line drawings
	- image derivatives (1st and 2nd order)
- object instance identification using color histograms
- performing evaluation

probabilità dadi
$Px(5) = 1/6$
$Py(5) = 1/6$
$Px+y(5) = ?$

We can count the possible cases
total cases: $6*6=36$


| 6   | 7   | 8   | 9   | 10  | 11  | 12  |
| --- | --- | --- | --- | --- | --- | --- |
| 5   | 6   | 7   | 8   | 9   | 10  | 11  |
| 4   | 5   | 6   | 7   | 8   | 9   | 10  |
| 3   | 4   | 5   | 6   | 7   | 8   | 9   |
| 2   | 3   | 4   | 5   | 6   | 7   | 8   |
| 1   | 2   | 3   | 4   | 5   | 6   | 7   |
|     | 1   | 2   | 3   | 4   | 5   | 6   |

possible cases: $P(3)P(1)+P(2)P(2)+P(1)P(3)$


$P[x*y](S) = $