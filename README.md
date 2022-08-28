# Easy Tennessee Eastman Challenge Simulator in Python

Hi there. So you might be looking to run the chemical plant from python
and don't want to shell out the money for matlab. Well, you are in
luck. Just call the fortran from python!

(Credits go to the original author.)[https://depts.washington.edu/control/LARRY/TE/download.html#Basic_TE_Code]

Of course if you want the plotting capabilities of the newer releases and want to spend the 
money on matlab, then (pytep)[https://github.com/ChristopherReinartz/pytep] is a fine alternative.

# Installation

`f2py3 -c -m tepy te.pyf teprob.f`

(te.pyf was generated from the [tutorial here](https://numpy.org/devdocs/f2py/f2py-examples.html))

# Example Usage 

```python
#!/usr/bin/env python3
import tepy

# initialize the plant and read the reactor pressure
time, yy, yp = tepy.teinit()
print(round(tepy.pv.xmeas[6],1), end=' ')

# set delta_t (bigger values are less accurate)
delta_t = 0.001

# wait ten minutes
for i in range(int((10/60)/delta_t)):
  yp = tepy.tefunc(nn=50, time=delta_t, yy=yy)
  yy += yp * delta_t
  print(round(tepy.pv.xmeas[6],1), end=' ')
print()

print('close the purge valve wait an hour (see teprob.f)')
# reactor pressure increases
tepy.pv.xmv[5] = 0.0
for i in range(int(1/delta_t)):
  yp = tepy.tefunc(nn=50, time=delta_t, yy=yy)
  yy += yp * delta_t
  # See the new reactor pressure
  print(round(tepy.pv.xmeas[6],1), end=' ')
```
