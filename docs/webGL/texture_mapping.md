





## perspective projection

![perspective_projection](images/perspective_projection.png)

bi

### clipping

![clipping](images/clipping.png)





## pinhome camera model

- image point (成像点)
- scene point ()
- image plane(成像平面,such as a camera sensor or the human retina)

![pinhole_camera](images/pinhole_camera.png)

![projection_transform](images/projection_transform.png)

perpsective effects

- converging lines + vanishing points
- closer objects appear larger in images
- blur effect
- sampling
- antialiasing(反锯齿)



### viewing frustum

![view_frustum.png](images/view_frustum.png)

- `clipping` eliminates triangles not visible to the camera



paramtererized by

- fovy
- aspect ratio
- near (depth of near cliping plane)
- far (depth of far cliping plane)



## Texture 

> wrap around the surface of geometry

![world_map_texture](images/world_map_texture.png)

![wrapper_sandan](images/wrapper_sandan.png)



`texture`: describe  surface material  property

![texture](images/texture.png)

surface material property beyond color

- wet/dry area of the surface
- bump map （凹凸）
- displacement(offset) map 
- light and shadow









![magnification_vs_minification](images/magnification_vs_minification.png)





![manification](images/manification.png)

trilinear filtering

![trilinear_filtering](images/trilinear_filtering.png)





Each surface point is assigned to texture coordinate(u,v)



> In GPUs, texture = memory + filtering



use for texturing

- enverionment lighting

![env_map_sphere](images/env_map_sphere.png)

![env_map_sphere_2](images/env_map_sphere_2.png)

- cubeMap  
- displacement map (位移)
- 
- ambient occlusion(环境光遮挡) texture map
- store microgeometry
- procedural textures
- solid modeling
- volume rendering



sampling(采样)

![point_sample](images/point_sample.png)



- 1:1 mapping

- Texture Magnification

- Texture Minification

## 数据拟合,逼近和插值

> fitting approximating interpolation

nearest neightbour interpolation



![fitting_func](images/fitting_func.png)



![interpolation](images/interpolation.png)

分类拟合

![fitting_func](images/fitting_func_find.png)



![fitting_data_set](images/fitting_data_set.png)





Texture Filtering

Texture Magnification

Texture Minification

Mipmap Limitations

Anisotropic sampling

Environment Map

Environmental Lighting

Cube Map

Displacement Mapping

Bump Mapping