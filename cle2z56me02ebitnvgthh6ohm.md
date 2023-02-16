# Resolving React Input Losing Focus with Unique Keys: The Impact of array Index vs Nanoid()/uuid()

## Losing Focus on Keypress

An issue that can arise when using a dynamic component in React is that the input can lose `focus` when a key is pressed.

<iframe src="https://share.descript.com/embed/sprryq1ov7X" width="300" height="320"></iframe>

This can be a frustrating user experience, as the user has to click back into the `input` field to continue typing.

The cause of this issue is that the `component` is being re-created each time a key is pressed, which leads to the input losing focus.

![function showing nanoid as the key prop for an array of elements](https://cdn.hashnode.com/res/hashnode/image/upload/v1676214094687/59311fdc-366c-47bb-933b-5f0357b75193.png align="center")

## Problem

When it comes to rendering lists of components in React, it's important to provide a unique "key" prop for each component. The key helps React keep track of each component and ensures that updates are efficiently managed when components are added, removed, or reordered.

### Using Array Index

One common way to generate keys is to use the `index` of each item in the array, but this approach can lead to bugs if the list is reordered or if new items are added or removed from the list. In such cases, the index will change and React won't be able to accurately track the components.

### Using Unique Identifiers

On the other hand, using a unique identifier like `nanoid()` or `uuid()` can help ensure that each component is accurately tracked, even if the list is reordered or if items are added or removed. The unique ID stays the same, even if the index changes, and React can use it to accurately track the component.

However, in the case of dealing with the input losing focus on keypress, nanoid may not be the best solution

This is because the nanoid() is dynamically added to the elements of the mapped array, which in turn is updated on each keystroke. This thus makes the app to re-render, thereby enabling nanoid to give each element a unique random key.

## Solution

Thus, in this case, using the index as the key can work perfectly fine and provides a simple, straightforward solution. This is because React only needs the key to be unique among its siblings, not globally unique. If the list of items is not reordered or modified, using the index as the key can be a viable solution.

![the key is set a the index of the forms](https://cdn.hashnode.com/res/hashnode/image/upload/v1676214621770/ce659db3-2da6-4429-994c-89db20d6cc11.png align="center")

In the video below, the key adjustment ensured that the form focus remains sam.

<iframe src="https://share.descript.com/embed/80nktnrEsX6" width="300"" height="360"></iframe>

## Conclusion

However, it's always a good idea to use a unique ID when working with dynamic lists of components to ensure that React can accurately track each component, even if the list changes.

In conclusion, the choice between using an index or a unique ID as a key in React depends on the specific requirements of your application. In general, it's recommended to use a unique ID when working with lists of components, but using the index can be a viable solution if the list is not expected to change.