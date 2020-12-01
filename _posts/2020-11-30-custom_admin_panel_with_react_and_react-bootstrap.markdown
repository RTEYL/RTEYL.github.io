---
layout: post
title:      "Custom admin panel with React and react-bootstrap"
date:       2020-11-30 22:04:02 -0500
permalink:  custom_admin_panel_with_react_and_react-bootstrap
---


I was looking online for libraries that would help me build a nice and simple admin panel for my react ecommerce project. I was going to try using the [react-admin](https://github.com/marmelab/react-admin) framework but as I'm using a Rails Api backend with sqlite3  there wasn’t an already built data provider for me to use.  Instead of creating my own provider and changing the behavior of my database ( my app was mostly complete at this point ), I built my own.

To start I needed to have protected routes that only an admin can see or use, using the Route components render method from `react-router-dom` I can pass the routers history as props with this syntax:

```jsx
import { Route } from "react-router-dom";
<Route
 path="/admin"
 render={(props) => (
 	<AdminRoutes {...props} admin={this.props.user.admin} />
 )}
/>
```

and passing in an admin prop via connection to redux store that is true or false base on the logged in user

```jsx
const mapStateToProps = (state) => {
  return {
    user: state.users.user,
  };
};
```

rather than:

```jsx
<Route path='/admin' component={AdminRoutes}
```

so I can redirect if the logged in user manually types in the url

```jsx
!this.props.admin && this.props.history.push("/");
```

To continue using my bootstrap Navbar I used conditional rendering:

```jsx
// props.user also being from connection to store
{props.user.admin && (
            <Nav.Link eventKey="7" as={Link} to="/admin">
              Admin
            </Nav.Link>
          )}
```

Moving into the AdminRoutes Component

```jsx
<>
        <Route
          exact
          path="/admin"
          render={(props) => (
            <AdminContainer items={this.props.items} {...props} />
          )}
        />
        <Route
          exact
          path="/admin/items/create"
          render={(props) => <ItemCreate {...props} />}
        />
        <Route
          path="/admin/items/:itemId/edit"
          render={(props) => <ItemEdit {...props} />}
        />
        <Route
          exact
          path="/admin/users/create"
          render={(props) => <UserCreate {...props} />}
        />
        <Route
          path="/admin/users/:userId/edit"
          render={(props) => <UserEdit {...props} />}
        />
      </>
```

> Note: I pass items as props in the container to update the redux store on edit or delete whereas I'm not storing all the users from the Api in the store so I wont need to update the redux store for users as well.

Now to the admin container....

I use react-bootstraps Tabs component to separate Items and Users list and create a nice UI. I also added some create_new buttons that link to their respective routes. 

```jsx
import React from "react";
import { Button, Tab, Tabs } from "react-bootstrap";
import ItemsList from "../components/admin/ItemsList";
import UsersList from "../components/admin/UsersList";

const AdminContainer = (props) => {
  return (
    <Tabs defaultActiveKey="Items" id="admin-tab-pannel">
      <Tab eventKey="Items" title="Items">
        <Button
          onClick={() => {
            props.history.push('/admin/items/create');
          }}
          size="sm"
          variant="info">
          Create New Item
        </Button>
        <ItemsList {...props} />
      </Tab>
      <Tab eventKey="Users" title="Users">
        <Button
          onClick={() => {
            props.history.push('/admin/users/create');
          }}
          size="sm"
          variant="info">
          Create New User
        </Button>
        <UsersList {...props} />
      </Tab>
    </Tabs>
  );
};

export default AdminContainer;
```

For the individual lists I mapped through the data to create a table/grid layout 

```jsx
renderItems = () => {
    return this.props.items.map((i) => {
      return (
        <tbody key={i.id}>
          <tr>
            <td>
              {i.id}
              <br />
              <Button
                onClick={() => {
                  this.props.history.push(`/admin/items/${i.id}/edit`);
                }}
                size="sm"
                variant="warning">
                Edit
              </Button>
              <Button
                onClick={() => this.props.deleteItem(i)}
                size="sm"
                variant="danger">
                Delete
              </Button>
            </td>
            <td>{i.name}</td>
            <td>{i.brand}</td>
            <td>{i.category}</td>
            <td>{i.description}</td>
            <td>{i.price}</td>
            <td>{i.image}</td>
          </tr>
        </tbody>
      );
    });
  };

  render() {
    return (
      <Table responsive striped bordered>
        <thead>
          <tr>
            <th>id</th>
            <th>Item Name</th>
            <th>Brand Name</th>
            <th>Category</th>
            <th>Description</th>
            <th>Price</th>
            <th>Image Url</th>
          </tr>
        </thead>
        {this.renderItems()}
      </Table>
    );
  }
}

```

I also added buttons to edit and delete them individually with `deleteItem()` as a dispatch action that sends the request to the backend and updates the store asynchronously.

the final product looking like: 

<img src="https://i.ibb.co/D5tyC5T/Capture.png" alt="Capture" border="0" />

Following the same steps add as many tabs, columns, and rows as you need! You just need to send your requests to the desired Api endpoint. I use `fetch()` and send requests to the Api and dispatch actions to the redux store after handling the response: 

```jsx
export const deleteItem = (item) => {
  let configObj = {
    credentials: "include",
    method: "DELETE",
    headers: {
      "Content-Type": "application/json",
      Accept: "application/json",
    },
  };
  return (dispatch) => {
    fetch(`http://your_api_url/endpoint/${item.id}`, configObj)
      .then((resp) => resp.json())
      .then((json) => {
      	if(json.status === 'ok'){
        dispatch({ type: "DELETE_ITEM", payload: item })
        }else{
          dispatch({type: "ADD_ERROR", payload: json.errors})
        }
      });
  };
```

The forms to edit and create are pretty standard, I used some bootstrap with mine for styling

```jsx
import { Button, Form } from "react-bootstrap";
<Form onSubmit={this.handleSubmit}>
        <Form.Group controlId="itemName">
          <Form.Label>Item Name</Form.Label>
          <Form.Control
            placeholder="Item Name"
            type="text"
            name="name"
            value={name}
            onChange={this.handleChange}
          />
         <Button variant="success" type="submit">
          Confirm Changes
        </Button>
      </Form>
```

and `onSubmit` dispatches an action as well.

This was my first project with React/Redux and with a days worth of coding/debugging on this admin panel, I love its simplicity as well as being able to customize it to my own needs without large imports or clones.... exception (*Cough, Cough*) Bootstrap. 
