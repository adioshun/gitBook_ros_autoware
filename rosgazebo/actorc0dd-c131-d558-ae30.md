# 사람 추가 (Actor)

> [Gazebo Tutorial](http://gazebosim.org/tutorials?tut=actor), [SDF](http://sdformat.org/spec?ver=1.6&elem=actor) Element, [Sample Code](https://bitbucket.org/osrf/gazebo/raw/348b3de008a00cc7b73ad95ad1b8ce00c9d22024/worlds/actor.world
gazebo) 


## 정의 
- an animated model is called an actor. 
- Actors extend `common models`, adding animation capabilities.

## 종류 

- Skeleton animation: which is relative motion between links in one model:
- Motion along a trajectory: which carries all of the actor's links around the world, as one group

* 두 종류를 합쳐서 동시에 적용 가능 

## 기존 Model과 차이점 

Gazebo's actors are just like `models`, so you can put links and joints inside them as usual. 

The main differences are:
- Actors are always static (i.e. no forces are applied on them, be it from gravity or contact or anything else)
- Actors support skeleton animation imported from COLLADA and BVH files.
- Actors can have trajectories scripted directly in SDF.
- There can't be models nested inside actors, so we're limited to animated meshes, links and joints.

