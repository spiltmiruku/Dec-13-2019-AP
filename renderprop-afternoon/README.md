<img src="https://s3.amazonaws.com/devmountain/readme-logo.png" width="250" align="right">

# Project Summary

In this project, we'll be creating a project similar to what we created in the higher order component project. This one will be written as a render prop. It is important to remember that these are two different patterns to accomplish the same task. In this project we will be building a currency converter that will convert a foreign currency to USD.

## Step 1

### Summary

In this step, we'll set up our file structure to keep things organized.

### Instructions

- `Fork` and `clone` this project
- Run `npm install`
- Inside the `src` folder, create a `Components` folder.
- Open `src/Components`. Create two new Files:
  - CurrencyConverter.js
  - CurrencyDisplay.js

### Solution

<details>

<summary>File Structure</summary>

```
-- src
  -- Components
    -- CurrencyConverter.js
    -- CurrencyDisplay.js
  -- App.css
  -- App.js
  -- index.css
  -- index.js
```

</details>

## Step 2

### Summary

In this step, we'll create the skeleton for our render prop component (CurrencyConverter.js).

### Instructions

- Import React.
- Create a class component called `CurrencyConverter`
- The `CurrencyConverter` component will have some state

<details>

<summary> Detailed Instructions </summary>

<br />

Remember render props are another pattern for cross cutting concerns and making your code reusable, similar to a HOC.

We first want to create a class based component we will call `CurrencyConverter`. We chose a class based component because this will have some state involved.

</details>

### Solution

<details>

<summary> <code> src/Components/CurrencyConverter.js </code> </summary>

```jsx
class CurrencyConverter extends Component {
  state = {}
  render(){
    return (
      // SOON TO BE JSX
    )
  }
}
```

</details>

## Step 3

### Summary

In this step, we will create the boilerplate for our currency converter. This will include a drop down and buttons to increment and decrement the amount to convert.

### Instructions

- Set intial state for this component. We will need three keys: `currencyChosen : false`, `selectedCurrency: 'Select Currency'` and `amount: 0`.

```js
const currencyData = [
	{ name: 'Japanese Yen', symbol: '¥', rate: 113.6, id: 0 },
	{ name: 'British Pound', symbol: '£', rate: 0.77, id: 1 },
	{ name: 'Canadian Dollar', symbol: 'CAD', rate: 1.32, id: 2 },
	{ name: 'Mexican Peso', symbol: 'Mex$', rate: 20.41, id: 3 },
	{ name: 'Swiss Franc', symbol: 'Fr.', rate: 1.01, id: 4 }
]
```

- In `render` use the above array, `currencyData`. Map over it and dynamically create `<option>` elements for each obect in the array.
- Create a `<select>` element to hold the options created above along with a single default `<option>` with a value of 'Select Currency'.
- Create two `<button>` elements, one should have `+` as it's inner text and the other should be `-`.
- Below the `button`'s, wrap `this.props.render()` in curly braces, passing in the object that represents the selected currency and the `amount` from state

<details>

<summary> Detailed Instructions </summary>

<br />

- Set some intial state for this component. We will need a `currencyChosen` which will default to `false`, `selectedCurrency` which will default as 'Select Currency' (spelling and capitalization are important here) and finally an `amount` with the default of `0`.

```js
const currencyData = [
	{ name: 'Japanese Yen', symbol: '¥', rate: 113.6, id: 0 },
	{ name: 'British Pound', symbol: '£', rate: 0.77, id: 1 },
	{ name: 'Canadian Dollar', symbol: 'CAD', rate: 1.32, id: 2 },
	{ name: 'Mexican Peso', symbol: 'Mex$', rate: 20.41, id: 3 },
	{ name: 'Swiss Franc', symbol: 'Fr.', rate: 1.01, id: 4 }
]
```

- Copy the above `currencyData` array inside of thhe `render()` method but outside of the `return` inside of the `CurrencyConverter` component.
- Using `.map()`, create an `<option>` element for each item of the `currencyData` array. Each `<option>` element should have a `key`, and `id` and have the individual currency name as text. call the new array `currencyOptions`.
- Create a container div `<div>`. Inside of the `div` create a `<select>` element.
- Inside of the `select` create a single `<option>` element with a attribute of `value='Select Currency'` and 'Select Currency' as the inner text. Below that, inside of `{}` display the `currencyOptions`.
- Create a new `<div>` to hold buttons that will increment and decrement the currency amount.
- Inside of the newly created `div`, create two `<button>` elements, one should have `+` as it's inner text and the other should be `-`. The button with the `+` should have a className of `add` and the button with the `-` should have a className of `minus`
- Below the `button`'s, render `this.props.render()` in curly braces. This is the function we will use to display our soon to be created component. The `this.props.render()` will have two arguments; one which will use `selectedCurrency` from state as the index to select an option from the `currencyData` array and the other argument will be called `amount` which will be the value of amount on state.
  </details>

### Solution

<details>

<summary> <code> src/Components/CurrencyConverter.js </code> </summary>

```jsx
import React, { Component } from 'react'

class CurrencyConverter extends Component {
	state = {
		currencyChosen: false,
		selectedCurrency: 'Select Currency',
		amount: 0
	}

	render() {
		const currencyData = [
			{ name: 'Japanese Yen', symbol: '¥', rate: 113.6, id: 0 },
			{ name: 'British Pound', symbol: '£', rate: 0.77, id: 1 },
			{ name: 'Canadian Dollar', symbol: 'CAD', rate: 1.32, id: 2 },
			{ name: 'Mexican Peso', symbol: 'Mex$', rate: 20.41, id: 3 },
			{ name: 'Swiss Franc', symbol: 'Fr.', rate: 1.01, id: 4 }
		]
		const currencyOptions = currencyData.map((currency, index) => (
			<option key={currency.id} value={index}>
				{currency.name}
			</option>
		))
		return (
			<div>
				<select value={this.state.selectedCurrency}>
					<option value='Select Currency'>Select Currency</option>
					{currencyOptions}
				</select>
				<div>
					<button className='add'>+</button>
					<button className='minus'>-</button>
				</div>
				{this.props.render(
					currencyData[this.state.selectedCurrency],
					this.state.amount
				)}
			</div>
		)
	}
}
```

</details>

## Step 4

### Summary

In this step, we'll create three methods to help us handle user interactions. We will be using the auto-binding (public class field syntax for these methods).

### Instructions

- Using the public class field syntax, create a method that will increment the count of amount on state. Use `setState` via the callback function syntax. Call this method `handleAmountIncrease`.
- Using the public class field syntax, create a method that will decrement the count of amount on state. Use `setState` via the callback function syntax. Call this method `handleAmountDecrease`.
- Using the public class field syntax, create a method that will handle the users selection from the dropdown. You will need to store their selection in a variable we will call `userValue`. This should update the `selectedCurrency` and `currencyChosen` properties on state. Call this method `handleOptionSelect`.
- Assign these functions to the appropriate elements and event handlers.
- Export the function to be used in App.js
- Import the `CurrencyConverter` component into `App` and render it, giving it a `render` prop

<details>

<summary> Detailed Instructions </summary>

<br />

- Let's start off by creating a method that will handle the increase of the amount on state. We will be using the public class field syntax and using `setState` with a callback, rather than a object. The callback will take one parameter, `prevState`. This parameter gives us access to state without modifying it directly.

```jsx
handleAmountIncrease = () => {
	this.setState((prevState) => {
		return {
			amount: prevState.amount + 1
		}
	})
}
```

- Next we will create a method that will handle the decrease of the amount on state. We will be using the public class field syntax and using `setState` with a callback for this as well. The callback will take one parameter, `prevState`. This parameter gives us access to state without modifying it directly.

```jsx
handleAmountDecrease = () => {
	this.setState((prevState) => {
		return {
			amount: prevState.amount - 1
		}
	})
}
```

- Finally we will create a method that will handle the user selection from the dropdown. We will be using the public class field syntax and using `setState` with a callback for this as well. The callback will take one parameter, `prevState`. This parameter gives us access to state without modifying it directly. This method will need an event passed into it. We will assign the value from `evt.target.value` to a variable we will call `userValue`. This `setState` won't need access to the `prevState` parameter but we will still use the callback syntax. Return a new object from `setState` that updates `selectedCurrency` and `curencyChosen` on state. The new value of `selectedCurrency` will be the `userValue` variable. The new value of `currencyChosen` will be a boolean. Using a ternary, determine if `userValue` is equal to 'Selected Currency' (punctuation matters here). If it does, set the value to `false`, otherwise set to `true`.

```jsx
handleOptionSelect = (evt) => {
	const userValue = evt.target.value
	this.setState(() => {
		return {
			selectedCurrency: userValue,
			currencyChosen: userValue === 'Select Currency' ? false : true
		}
	})
}
```

- Last step is to use these methods in the appropriate spots.
  - Using an `onClick` event listener, use the `handleAmountIncrease` method on the button with a `+` as the inner text.
  - Using an `onClick` event listener, use the `handleAmountDecrease` method on the button with a `-` as the inner text.
  - Using an `onChange` event listener, use the `handleOptionSelect` method on the select element.
- We will also export our component so we can use it on `App.js`.
- Clear out all of the JSX from Create React app.
- Render the new `CurrencyConverter` component. It will need one prop called `render`. Remember, this prop can be called anything but it is common convention to call it `render`. This prop will contain an anonymous arrow function. Use a fragment as the container div.

</details>

### Solution

<details>

<summary> <code> src/Components/CurrencyConverter.js </code> </summary>

```jsx
import React, { Component } from 'react'

class CurrencyConverter extends Component {
	state = {
		currencyChosen: false,
		selectedCurrency: 'Select Currency',
		amount: 0
	}

	handleAmountIncrease = () => {
		this.setState((prevState) => {
			return {
				amount: prevState.amount + 1
			}
		})
	}

	handleAmountDecrease = () => {
		return (
			this.state.amount > 0 &&
			this.setState((prevState) => {
				return {
					amount: prevState.amount - 1
				}
			})
		)
	}

	handleOptionSelect = (evt) => {
		const userValue = evt.target.value
		this.setState(() => {
			return {
				selectedCurrency: userValue,
				currencyChosen: userValue === 'Select Currency' ? false : true
			}
		})
	}

	render() {
		const currencyData = [
			{ name: 'Japanese Yen', symbol: '¥', rate: 113.6, id: 0 },
			{ name: 'British Pound', symbol: '£', rate: 0.77, id: 1 },
			{ name: 'Canadian Dollar', symbol: 'CAD', rate: 1.32, id: 2 },
			{ name: 'Mexican Peso', symbol: 'Mex$', rate: 20.41, id: 3 },
			{ name: 'Swiss Franc', symbol: 'Fr.', rate: 1.01, id: 4 }
		]
		const currencyOptions = currencyData.map((currency, index) => (
			<option key={currency.id} value={index}>
				{currency.name}
			</option>
		))
		return (
			<div>
				<select
					value={this.state.selectedCurrency}
					onChange={this.handleOptionSelect}>
					<option value='Select Currency'>Select Currency</option>
					{currencyOptions}
				</select>
				<div>
					<button className='add' onClick={this.handleAmountIncrease}>
						+
					</button>
					<button className='minus' onClick={this.handleAmountDecrease}>
						-
					</button>
				</div>
				{this.props.render(
					currencyData[this.state.selectedCurrency],
					this.state.amount
				)}
			</div>
		)
	}
}

export default CurrencyConverter
```

</details>

<details>

<summary> <code> src/App.js </code> </summary>

```jsx
import React, { Component } from 'react'
import './App.css'

import CurrencyConverter from './Components/CurrencyConverter.js'

class App extends Component {
	render() {
		return (
			<>
				<h2>Render Props</h2>
				<CurrencyConverter render={() => {}} />
			</>
		)
	}
}

export default App
```

</details>

## Step 5

### Summary

In this step, we'll write some JSX to display the exchanged currency.

### Instructions

- Staying in `App.js`, pass in two parameters to our `render prop` function; `currencyData` and `amount`. These will represent `currencyData[this.state.selectedCurrency]` and `this.state.amount` in a render function in `CurrencyConverter.js`.
- Return a `<p>` element that will hold the text displaying the amount in US dollars, the name of the currency we are exchanging to, the symbol and the exchanged amount.

<details>

<summary> <code> Detailed Instructions </code> </summary>

<br />

- Take a look at our render prop on the CurrencyConverter Component. It is an anonymous function. We know that functions can return anything and in this case we will be returning some JSX. Before that though, we need to pass in two parameters that will represent our data when we invoke our render prop function in `CurrencyConverter.js`.
- Have our render prop function return a `p` tag.
- The `p` element should show the US Dollar amount, the name of the currency, the symbol and then the amount of the exchanged currency.
- If having this `p` element be represented as a new component, you can create a new component with the data passed down as a prop. The new component will be returned from the render prop rather than JSX.
- Add in a `<h2>` with some text signaling that this is a render prop version of this project. This will help you when you are comparing render props to HOC's

</details>

### Solution

<details>

<summary> <code> src/App.js </code> </summary>

```jsx
import React, { Component } from 'react'
import './App.css'

import CurrencyConverter from './Components/CurrencyConverter'

class App extends Component {
	render() {
		return (
			<>
				<h2>Render Props</h2>
				<CurrencyConverter
					render={(currencyData, amount) => (
						<p>
							US Dollar ${amount.toFixed(2)} - {currencyData.name}{' '}
							{currencyData.symbol}
							{(amount * currencyData.rate).toFixed(2)}
						</p>
					)}
				/>
			</>
		)
	}
}

export default App
```

</details>

## Step 6

### Summary

In this step we will refactor the JSX you wrote in `App` in the previous step into a reusable component in `CurrencyDisplay`. We will then replace that JSX with the newly created `CurrencyDisplay` component.

### Instructions

- Create a stateless, functional component in `CurrencyDisplay.js`
- Have it return the JSX that we created in `App`, getting it's values from props
- Replace the JSX returned from the render prop in `App` to be the `CurrencyDisplay` component

<details>

<summary> <code> Detailed Instructions </code> </summary>

<br />

- In `CurrencyDisplay.js` import React. Without having React in scope, we cannot use JSX.
- Create a function expression called `CurrencyDisplay` using arrow function syntax. Have it accept `props` as its only parameter
- Return a `<p>` tag that contains the same JSX as what we have in the `<p>` tag in `App`. Access the `currencyData` and `amount` values off of the `props` object.
- Export `CurrencyDisplay` by default from `CurrencyDisplay.js`
- In `App`, import the `CurrencyDisplay` component from `CurrencyDisplay.js`
- Delete the `<p>` tag being returned by the `render` prop of `CurrencyConverter`. Replace it, returning an instance of the `CurrencyDisplay` component instead. Pass the `CurrencyDisplay` component props of `currencyData` and `amount` that are equal to the parameters of the same name being taken in to your `render` prop callback function.

</details>

### Solution

<details>

<summary> <code> src/Components/CurrencyDisplay.js </code> </summary>

```jsx
import React from 'react'

const CurrencyDisplay = (props) => (
	<p>
		US Dollar ${props.amount.toFixed(2)} - {props.currencyData.name}{' '}
		{props.currencyData.symbol}
		{(props.amount * props.currencyData.rate).toFixed(2)}
	</p>
)

export default CurrencyDisplay
```

</details>

<details>

<summary> <code> src/App.js </code> </summary>

```jsx
import React, { Component } from 'react'
import './App.css'

import CurrencyConverter from './Components/CurrencyConverter'
import CurrencyDisplay from './Components/CurrencyDisplay'

class App extends Component {
	render() {
		return (
			<>
				<h2>Render Props</h2>
				<CurrencyConverter
					render={(currencyData, amount) => (
						<CurrencyDisplay currencyData={currencyData} amount={amount} />
					)}
				/>
			</>
		)
	}
}

export default App
```

</details>

## Step 7

### Summary

You may have noticed by now that our project doesn't run yet and we are getting an error `Cannot read 'name' of undefined`. Think for a moment about what may be causing this problem?

This is happening because we are trying to display our data before we have anything selected from the dropdown. In this step we will conditionally render text if the user hasn't selected an option from the dropdown.

### Instructions

- Inside of `CurrencyConverter.js`, look to the return statement and find where we are invoking `this.props.render`. We will need to conditionally render this function only if a user has selected something from the dropdown. If the user has not, we will display the text `Please Select Currency`.

<details>

<summary> Detailed Instructions </summary>

<br />

Head down to the return of the `CurrencyConverter` component. Here we will use a ternary to determine if our user has selected something from the dropdown. Luckily on state we have a key of `currencyChosen` which is a boolean. Use this to determine if we should display the result of invoking `this.props.render` or if we should display the text `Please Select Currency`.

</details>

### Solution

<details>

<summary> <code> src/Components/CurrencyConverter.js </code> </summary>

```jsx
import React, { Component } from 'react'
import CurrencyDisplay from './CurrencyDisplay'

class CurrencyConverter extends Component {
	state = {
		currencyChosen: false,
		selectedCurrency: 'Select Currency',
		amount: 0
	}

	handleOptionSelect = (evt) => {
		const userValue = evt.target.value
		this.setState(() => {
			return {
				selectedCurrency: userValue,
				currencyChosen: userValue === 'Select Currency' ? false : true
			}
		})
	}
	// OTHER STEPS
	render() {
		// OTHER STEPS
		return (
			<div>
				{/*OTHER STEPS*/}
				{this.state.currencyChosen ? (
					this.props.render(
						currencyData[this.state.selectedCurrency],
						this.state.amount
					)
				) : (
					<p>Please Select Currency</p>
				)}
			</div>
		)
	}
}

export default CurrencyConverter
```

</details>

## Black Diamond ◆

### Summary

This step is meant to stretch what you know and combine your knowledge. The first part of the black diamond is to make it so the user cannot go lower than 0. The second part of the challenge is to disable the buttons until an option from the dropdown has been selected.

The final challenge of the black diamond is to make it so every time a user selects a new currency it outputs a new currency display card rather than updating the same card each time.

## Contributions

If you see a problem or a typo, please fork, make the necessary changes, and create a pull request so we can review your changes and merge them into the master repo and branch.

## Copyright

© DevMountain LLC, 2017. Unauthorized use and/or duplication of this material without express and written permission from DevMountain, LLC is strictly prohibited. Excerpts and links may be used, provided that full and clear credit is given to DevMountain with appropriate and specific direction to the original content.

<p align="center">
<img src="https://s3.amazonaws.com/devmountain/readme-logo.png" width="250">
</p>
