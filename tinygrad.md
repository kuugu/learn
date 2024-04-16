stride: 
- the number of jumps required to go to the next element in the specified dimension. 
- contiguiity is dependent on the underlying 1D storage block 

```python 
from tinygrad.tensor import Tensor 

a = Tensor.rand(3, 4) 
b = a.permute(1, 0) 

a.lazydata.st.views # (View(shape=(3, 4), strides=(4, 1), offset=0, mask=None, contiguous=True),)
b.lazydata.st.views # (View(shape=(4, 3), strides=(1, 4), offset=0, mask=None, contiguous=False),)
```

tinygrad.engine.schedule.create_schedule: 
- realizes seems to be list of buffers that need to be realized. 

