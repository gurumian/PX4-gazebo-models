# Follow-sim pedestrian meshes

`MaleVisitorWalk`, `FemaleVisitorWalk`, and `VisitorKidWalk` ship COLLADA walk
animations under `models/*/meshes/` so `worlds/follow.sdf` uses `model://` URIs
and Gazebo does not download from Fuel at sim start (~2 MB total in git).

To refresh from Fuel (optional):

```bash
gz fuel download -u 'https://fuel.gazebosim.org/1.0/OpenRobotics/models/Male%20visitor' -v tip
gz fuel download -u 'https://fuel.gazebosim.org/1.0/Luca/models/FemaleVisitorWalk' -v tip
gz fuel download -u 'https://fuel.gazebosim.org/1.0/Luca/models/VisitorKidWalk' -v tip
# then copy from ~/.gz/fuel/.../meshes/*.dae into the matching models/*/meshes/
```
