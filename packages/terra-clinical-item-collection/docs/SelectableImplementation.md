# Selectable Implementation

The Item Collection provides the foundation to be rendered as a selectable component when a selection event is provided to the collection and items are indicated as selectable. The selection state management is then controlled by the component rendering the Item Collection. When an item receives a click or keydown event, the event and item key is returned such that the desired selection logic can be applied before updating the item's selection state. This means that the Item Collection is capable of being a single- or multi- selectable component.

### Implementation Details
To create a Selectable Item Collection, add the following to a static Item Collection implementation:
1. Pass an `onSelect` function to the ItemCollection. This function will handle selection state updates.
2. Add the `isSelectable` prop to each Item that should have selectable behavior.
    1. When adding `isSelectable`, the following attributes are added to the item: hover and focus styles, tabIndex=0, onClick and onKeyDown events. These events use the onSelect function passed to the ItemCollection.
3. Add a unique key to each item. These keys should be managed to handle selection state.
4. Add the `isSelected` prop to the items that have a selected state.
    1. When adding `isSelected`, the selected styles are applied.

### Selectable Item Collection Example

```diff
import React from 'react';
import ItemCollection from 'terra-clinical-item-collection';

class SelectableExample extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
+     selectedKey: //add logic that determines the selected index
    };
+   this.handleOnSelect = this.handleOnSelect.bind(this);
  }

+ handleOnSelect(event, selectedKey) {
+   if ( // add logic to determine if selection should occur ) {
+     event.preventDefault();
+     this.setState({ selectedKey });
+   }
+ }

  render() {
    return (
      <ItemCollection
        breakpoint="tiny"
        hasStartAccessory
        numberOfDisplays={3}
        hasComment
        hasEndAccessory
+       onSelect={this.handleOnSelect}                      // Step 1
      >
        <ItemCollection.Item
+         isSelectable                                      // Step 2
+         key="key-1"                                       // Step 3
+         isSelected={this.state.selectedKey === 'key-1'}   // Step 4
          startAccessory={<Icon/>}
          comment={<ItemCollection.Comment text="Comment" />}
          endAccessory={<Icon/>}
        >
          <ItemCollection.Display text="Display 1" />
          <ItemCollection.Display text="Display 2" />
          <ItemCollection.Display text="Display 3" />
        </ItemCollection.Item>
        <ItemCollection.Item
+         isSelectable                                      // Step 2
+         key="key-2"                                       // Step 3
+         isSelected={this.state.selectedKey === 'key-2'}   // Step 4
          startAccessory={<Icon/>}
          comment={<ItemCollection.Comment text="Comment" />}
          endAccessory={<Icon/>}
        >
          <ItemCollection.Display text="Display 1" />
          <ItemCollection.Display text="Display 2" />
        </ItemCollection.Item>
      </ItemCollection>
    );
  }
}

export default SelectableExample;
```
