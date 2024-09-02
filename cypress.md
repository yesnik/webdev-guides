# Cypress

Docs: https://docs.cypress.io/

Run Tests Runner: 

```bash
npx cypress open
```

## API Test

File: `cypress/e2e/api/http_methods.cy.js`:

```js
describe('HTTP Request', () => {
	it('GET method', () => {
		cy.request('GET','https://my-json-server.typicode.com/typicode/demo/posts')
		.then((response) => {
			console.log(response.body)
			expect(response.body).to.deep.eq([{id:1,title:"Post 1"},{id:2,title:"Post 2"},{id:3,title:"Post 3"}])
		})
	})

	it ('POST method', () => {
		cy.request({
			method: 'POST',
			url: 'https://my-json-server.typicode.com/typicode/demo/posts',
			body: {
				title: 'Post 555'
			}
		})
		.then((response) => {
			expect(response.status).to.eq(201)
			
		})
	})
})
```
