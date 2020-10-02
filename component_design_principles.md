# Component Design Principles

## 1. Component Cohesion Principles

They focus on the granularity of components and help the developer partition classes into components.

### 1.1. The Reuse/Release Equivalence Principle (REP)

The granular of reuse is the granular of release.

REP states that the granule of reuse, a component, can be no smaller than the granule of release. 
Anything that we reuse must also be released and tracked. It is not realistic for a developer to simply write a class and then claim that it is reusable. 
Reusability comes only after a tracking system is in place and offers the guarantees of notification, safety, and support that the potential reusers will need. 
REP gives us our first hint at how to partition our design into components. 
Since reusability must be based on components, reusable components must contain reusable classes. 
So, at least some components should comprise reusable sets of classes.

### 1.2. The Common Reuse Principle (CRP)

The classes in a component are reused together. If you reuse one of the classes in a component, you reuse them all.

This principle helps us to decide which classes should be placed into a component. 
CRP states that classes that tend to be reused together belong in the same component. Classes are seldom reused in isolation. 
Generally, reusable classes collaborate with other classes that are part of the reusable abstraction. CRP states that these classes belong together in the same component. 
In such a component, we would expect to see classes that have lots of dependencies on each other. 
A simple example might be a container class and its associated iterators. 
These classes are reused together because they are tightly coupled. Thus, they ought to be in the same component.

### 1.3. The Common Closure Principle (CCP)

The classes in a component should be closed together against the same kinds of changes. 
A change that affects a component affects all the classes in that component and no other components.

This is the Single-Responsibility Principle (SRP) restated for components. Just as SRP says that a class should not contain multiple reasons to change, 
CCP says that a component should not have multiple reasons to change. In most applications, maintainability is more important that reusability. 
If the code in an application must change, you would prefer the changes to occur all in one component rather than being distributed through many components. 
If changes are focused into a single component, we need redeploy only the one changed component. 
Other components that don't depend on the changed component do not need to be revalidated or redeployed.

## 2. Component Coupling Principles

They focus on the stability and relationships between the components.

### 2.1. The Acyclic Dependencies Principle (ADP)

Allow no cycles in the component dependency graph.

The dependency structure must always be monitored for cycles. When cycles occur, they must be broken somehow. 
Sometimes, this will mean creating a new component, making the dependency structure grow.

### 2.2. The Stable-Dependency Principle (SDP)

Depend in the direction of stability.

Designs cannot be completely static. Some volatility is necessary if the design is to be maintained. 
We accomplish this by conforming to CCP. Using this principle, we create components that are sensitive to certain kinds of changes. 
These components are designed to be volatile; we expect them to change. 
Any component that we expect to be volatile should not be depended on by a component that is difficult to change! 
Otherwise, the volatile component will also be difficult to change. 
It is the perversity of software that a module that you have designed to be easy to change can be made difficult to change by 
someone else simply hanging a dependency upon it. Not a line of source code in your module need change, 
and yet your module will suddenly be difficult to change. 
By conforming to SDP, we ensure that modules that are intended to be easy to change are 
not depended on by modules that are more difficult to change than they are.

### 2.3. The Stable-Abstractions Principle (SAP)

A component should be as abstract as it is stable.

This principle sets up a relationship between stability and abstractness. 
It says that a stable component should also be abstract so that its stability does not prevent it from being extended. 
On the other hand, it says that an instable component should be concrete, since its instability allows the concrete code within it to be easily changed. 
Thus, if a component is to be stable, it should also consist of abstract classes so that it can be extended. Stable components that are extensible.

Source: https://blog.avenuecode.com/principles-of-package-and-component-design
