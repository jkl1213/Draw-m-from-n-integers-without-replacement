# Draw-m-from-n-integers-without-replacement
Image we draw 5 numbers from 1 to 100 without replacement. 
Then we take the maximum number in the five number drawn minus the minimum number drawn.
What is the probability distribution of such a difference? 

### First, we import the relevant packages to help us calculate combinations
```
import scipy
from scipy import special
```
### There are two ways to think about this problem. The first way is as follows:
```
n = 100
m = 5
results = []
for k in range(m-1,n):
    order = (np.math.factorial(m)/(np.math.factorial(2)*np.math.factorial(m-2)))
    prob = order*((n-k)/scipy.special.binom(n,2))*(scipy.special.binom(k-1,m-2)/scipy.special.binom(n-2,m-2))
    results.append(prob)

import matplotlib.pyplot as plt
x = np.arange(m-1,n)
y = np.array(results)
plt.bar(x, y, color='grey');
plt.title("Probability distribution of maximum gap");
plt.xlim(m-1,n);
```
### We plot the probability distribution of maximum gap
<img width="509" alt="Screenshot 2021-07-26 at 7 05 30 PM" src="https://user-images.githubusercontent.com/79690350/126979143-f17dd67b-5d74-44cb-96ac-19997cbdaea0.png">

### The second way is much simpler. Both ways lead to the same result.
```
n = 100
m = 5
results = []
for k in range(m-1,n):
    prob = scipy.special.binom(k-1,m-2)*(n-k)/scipy.special.binom(n,m)
    results.append(prob)
```
### Again, we plot the probability distribution of maximum gap and see the two are equivalent
<img width="515" alt="Screenshot 2021-07-26 at 7 06 09 PM" src="https://user-images.githubusercontent.com/79690350/126979201-64adc726-c54d-4452-9bd0-7b93a4868a31.png">

### We check that the probabilities sum to one
```np.sum(results)```
### The mean is therefore obtained by summing xP(X=x)
```np.dot(results,x)```
### The median is obtained 
```
#median
p = 0
for i in range(len(results)+1):
    p += results[i]
    if p >= 0.5:
        print("Median: {}".format(i))
        break
```
### Finally, we run simulation to see if the empirical probability distribution is close to calculation
```
import random
n = n
m = m
res = []
rep = 5000
num = set(np.arange(1,n+1))
for i in range(rep):
    rand = random.sample(num, m)
    max_num = max(rand)
    min_num = min(rand)
    gap = max_num - min_num
    res.append(gap)
from collections import Counter
import collections
recounted = Counter(res)
dict(recounted)
od = collections.OrderedDict(sorted(dict(recounted).items()))
empirical_prob = []
gap = []
for k, v in od.items(): 
    empirical_prob.append(v/rep)
    gap.append(k)
```
### Plotting the results of 5000 draws
```
import matplotlib.pyplot as plt
x = np.array(gap)
y = np.array(empirical_prob)
plt.bar(x, y, color='grey');
plt.title("Probability distribution of maximum gap");
plt.xlim(m-1,n);
```
<img width="528" alt="Screenshot 2021-07-26 at 7 10 12 PM" src="https://user-images.githubusercontent.com/79690350/126979710-1e1e0cbb-ae41-4127-b19f-86363f835c46.png">


