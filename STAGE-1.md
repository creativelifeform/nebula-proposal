# Stage 1

Author(s): [rohan-deshpande](http://github.com/rohan-deshpande)

Last updated: 2018-11-17

Status: **In Progress**

## [Stage 0](STAGE-0.md)

### Outcomes

* **Particle Engine** -  [`three.proton`](https://github.com/rohan-deshpande/three.proton) has been chosen as the particle engine
* **UI Toolkit** - [`blueprintjs`](https://blueprintjs.com/docs/) has been chosen as the UI toolkit
* **License** - This app will be open source and licensed under the MIT license

### Outstanding

* [Sequences](STAGE-0.md#sequences)
* [Collisions](STAGE-0.md#collisions)
* [Electron vs NWJS](STAGE-0.md#electron-vs-nwjs)

## Abstract

With the particle engine, UI toolkit and license for the application established, this stage will aim to detail the specific features which the GUI for the application should expose to users. Without this exploratory stage, it will be difficult to design and organise the UI in an intuitive, user friendly and scalable manner.

## Proposal

Much of the GUI features will be closely coupled to the chosen particle engine, `three.proton` as controls will have to be designed to directly interact with this library's API.

Using the `proton` [documentation](https://projects.jpeer.at/proton/index.html) we can easily extract functionality which will be key for a GUI to be able to mutate, control and ultimately design a complex particle system.

## Rationale

While I am slightly concerned about coupling the application to a specific particle engine, I don't see any real alternatives here short of building our own engine from scratch - and this is not something I want to spend time doing. At least `three.proton` is open source, MIT licensed and not overly complex, so if we need to change anything or extend it in any way, it's easy to do.

## Implementation

### Levels of Control

We can split the levels of control into three levels

* **High Level** - Adding / removing particle emitters in a "layer" style interface. See [Emitter](#emitter) for more info.
* **Mid Level** - Configuring the emitters themselves. To achieve this, a specific emitter must be selected, doing so should expose the controls for editing an emitter. See [Emitter Properties](#emitter-properties) for more info.
* **Low Level** - Configuring an emitters' behavioural properties. To achieve this, a specific emitter's specific behaviour will need to be selected. Doing so should expose controls for editing that behaviour's internal properties. See [Behaviour Properties](#behaviour-properties) for more info.


### UI / UX

#### Components

##### Header

The `Header` component will render the title of the particle system.


##### Emitters

The `Emitters` component should render all `Emitter` components of the particle system being designed in a layers style view. It should provide the following actions

* Add - Adds an emitter

##### Emitter

The `Emitter` component should provide some basic actions for interacting with a particle emitter such as

* Remove - Removes the emitter
* Select - Selects the emitter, enabling editing of the emitter properties in the `Properties` component. [See the Emitter Properties](#emitter-properties) section for more info.
* Expand - Expands the emitter to show its behaviours
* Toggle Pause / Play - Toggles the playing of the emitter in the `Stage` component
* Toggle Visibility - Toggles the visibility of the emitter in the `Stage` component

Each `Emitter` should have a name which is content editable.

##### Behaviour

The `Behaviour` component should become available after an `EXPAND` action has been fired for an `Emitter`. Selecting a behaviour should inject its current properties into the `Properties` component and enable the editing of these properties.

##### Stage

The `Stage` component is where Three's WebGL context is injected and the particle system is displayed. It should provide

* Orbital controls to allow the user to easily inspect every aspect of the particle system they are designing

##### Properties

The `Properties` component should be a reactive component which provides the bulk of editing functionality for emitters and their behaviours.

###### Emitter Properties

When an `Emitter` is selected, the following properties must be editable

* Name `:string`
* Zone type `:string`
* Zone properties (differs according to Zone type)
* Number of particles to emit `:integer`
* Rate of particles to emit `:number`
* Texture `:string`

###### Behaviour Properties

When a `Behaviour` is selected the following common properties must be editable

* Life `:integer`
* Easing `:string`

Each behaviour has a number of different properties that must be editable, I won't list them all here, for that we should reference the documentation

* [Alpha](https://projects.jpeer.at/proton/Proton_Proton.Alpha.html)
* [Attraction](https://projects.jpeer.at/proton/Proton_Proton.Attraction.html)
* [Collision](https://projects.jpeer.at/proton/Proton_Proton.Collision.html)
* [Color](https://projects.jpeer.at/proton/Proton_Proton.Color.html)
* [CrossZone](https://projects.jpeer.at/proton/Proton_Proton.CrossZone.html)
* [Force](https://projects.jpeer.at/proton/Proton_Proton.Force.html)
* [Gravity](https://projects.jpeer.at/proton/Proton_Proton.Gravity.html)
* [GravityWell](https://projects.jpeer.at/proton/Proton_Proton.GravityWell.html)
* [RandomDrift](https://projects.jpeer.at/proton/Proton_Proton.RandomDrift.html)
* [Repulsion](https://projects.jpeer.at/proton/Proton_Proton.Repulsion.html)
* [Rotate](https://projects.jpeer.at/proton/Proton_Proton.Rotate.html)
* [Scale](https://projects.jpeer.at/proton/Proton_Proton.Scale.html)

**NOTE!** The docs above are for `proton` NOT `three.proton`, so some properties may vary. Check [https://github.com/rohan-deshpande/three.proton/tree/master/src/behaviour](https://github.com/rohan-deshpande/three.proton/tree/master/src/behaviour) for accurate properties.
