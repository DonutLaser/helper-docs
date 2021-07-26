# Physics

### Raycasting

To raycast, use

```python
var space_state = get_world().direct_space_state
var result = space_state.intersect_ray(from, to)
```

**NOTE**: `to` argument is not a direction of the ray, but rather the endpoint

or use `Raycast` node as you can request the node to update the raycast anytime you want without waiting for a physics step.

