# Javascript Design Patterns

## Introduction

I'll be adding more and more patterns over time which you can use as a reference for your own JavaScript apps.

### Module Pattern (using IIFE's)

```javascript
const ATM = (function (){
	let credit = 0;

	function depositPrivate(n) {
		credit += n;
	}

	return {
		deposit: function (n) {
			depositPrivate(n);
			console.log(`Hey! Thanks for depositing ${n} in our bank.\n` +
				`The deposit has been successfully processed.`);
		},
		
		show: function () {
			console.log(`Your credit: ${credit}`);
		}
	}
})()
```

### Module Pattern (using Arrow functions)

```javascript
const ATM = (() => {
	let credit = 0;

	const depositPrivate = n => {
		credit += n;
	}

	return {
		deposit: n => {
			depositPrivate(n);
			console.log(
					`Hey! Thanks for depositing ${n} in our bank.\n` +
					`The deposit has been successfully processed.`);
		},
		
		show: () => {
			console.log(`Your credit: ${credit}`);
		}
	}
})()
```

### Module Pattern using file encapsulation (using NodeJS module.exports)

// ATM.js

```javascript
let credit = 0;

const depositPrivate = n => {
	credit += n;
}

//nodeJS
module.exports = {
	deposit: n => {
		depositPrivate(n);
		console.log(
				`Hey! Thanks for depositing ${n} in our bank.\n` +
				`The deposit has been successfully processed.`);
	},
	
	show: () => {
		console.log(`Your credit: ${credit}`);
	}
}
````

// index.js

```javascript
const ATM = require('./ATM.js');

console.log(ATM) // object
ATM.show()     //? 0
ATM.deposit(5)
ATM.show()     //? 5
```