# SteelEye Frontend

## 1. Explain what the simple List component does.
The simple List component is a React component that takes an array of items as a prop and renders a list of those items with the ability to select an item from the list. It consists of two nested components: SingleListItem and ListComponent. SingleListItem is responsible for rendering a single list item, while ListComponent is responsible for rendering the entire list by mapping over the items array and rendering a SingleListItem for each item.

## 2. What problems / warnings are there with code?

1) In the WrappedSingleListItem, the onClickHandler is not being invoked properly. Instead of being called when the list item is clicked, the onClickHandler is being called immediately when the component is rendered. 

`onClick={onClickHandler(index)}`

2) Incorrect syntax of useState() hook. The useState() hook returns an array of two elements where the first element is the state value and the second element is a function to update the state value. The array elements need to be swapped.

`const [setSelectedIndex, selectedIndex] = useState();`

3) In the items.map() function, each child in a list should have a unique key prop. Also, the isSelected prop should be passed as a boolean, instead it is being passed as a selectedIndex state value. The correct code should be:

`{items.map((item, index) => (
        <SingleListItem
          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          isSelected={selectedIndex}
        />
      ))}`
      
4) In the WrappedListComponent, the prop type for the items array is not defined correctly. 
  
  `PropTypes.array(PropTypes.shapeOf({...}))`
  
5) The items in WrappedListComponent.defaultProps cannot be null. They must have an array of elements.
  
  `WrappedListComponent.defaultProps = {
  items: null,
};`

## 3. Please fix, optimize, and/or modify the component as much as you think is necessary.

```
import React, { useState, useEffect, memo } from 'react';
import PropTypes from 'prop-types';

// Single List Item
const WrappedSingleListItem = ({
  index,
  isSelected,
  onClickHandler,
  text,
}) => {
  return (
    <li
      style={{ backgroundColor: isSelected ? 'green' : 'red'}}
      onClick={()=> onClickHandler(index)}
    >
      {text}
    </li>
  );
};

WrappedSingleListItem.propTypes = {
  index: PropTypes.number,
  isSelected: PropTypes.bool,
  onClickHandler: PropTypes.func.isRequired,
  text: PropTypes.string.isRequired,
};

const SingleListItem = memo(WrappedSingleListItem);

// List Component
const WrappedListComponent = ({
  items,
}) => {
  const [selectedIndex, setSelectedIndex] = useState();

  useEffect(() => {
    setSelectedIndex(null);
  }, [items]);

  const handleClick = index => {
    setSelectedIndex(index);
  };

  return (
    <ul style={{ textAlign: 'left' }}>
      {items.map((item, index) => (
        <SingleListItem
          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          isSelected={index === selectedIndex}
        />
      ))}
    </ul>
  )
};

WrappedListComponent.propTypes = {
  items: PropTypes.arrayOf(PropTypes.shape({
    text: PropTypes.string.isRequired,
  })),
};

WrappedListComponent.defaultProps = {
  items: [{
    text: "Daksh Patidar 12019231",
  },
  {
    text: "SteelEye Frontend Assignment ",
  },
  {
    text: "SteelEye was established in October 2017 to reduce the complexity and cost of compliance and to enable financial firms globally to manage their regulatory obligations through a single platform.",
  },
  {
    text: "SteelEye is the first and only truly integrated surveillance solution. We empower you to focus on what matters, all from a single platform.",
  },
]
};

const List = memo(WrappedListComponent);

export default List;
```
