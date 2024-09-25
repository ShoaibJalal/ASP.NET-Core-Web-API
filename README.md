# ASP.NET Core Web API

## Overview

This project is a simple **ASP.NET Core Web API** that demonstrates the use of standard HTTP methods such as **GET**, **POST**, **PUT**, and **DELETE** to perform basic CRUD (Create, Read, Update, Delete) operations from an in-memory cache.

The API is built using **A Web API with ASP.NET Core Controllers**, providing a clean and organized structure to handle various HTTP requests and manipulate data.

## Features

- **GET**: Retrieve data from the server.
- **POST**: Add new data to the server.
- **PUT**: Update existing data on the server.
- **DELETE**: Remove data from the server.

### Technologies Used

- **ASP.NET Core 8**
- **C#**
- **In-memory data storage**

## Endpoints

| HTTP Verb | Endpoint         | Description                         |
|-----------|------------------|-------------------------------------|
| GET       | `/api/pizza`      | Retrieve all pizza                  |
| GET       | `/api/pizza/{id}` | Retrieve a single pizza by ID        |
| POST      | `/api/pizza`      | Add a new pizza                      |
| PUT       | `/api/pizza/{id}` | Update an existing pizza by ID       |
| DELETE    | `/api/pizza/{id}` | Delete an pizza by ID                |

## Project Structure

- **Controllers**: Contains the API controller that defines the endpoints.
- **Models**: Contains the data models.
- **Services**: Optional services for business logic (if needed).

### Example API Controller

```csharp
using ContosoPizza.Models;
using ContosoPizza.Services;
using Microsoft.AspNetCore.Mvc;

namespace ContosoPizza.Controllers;

[ApiController]
[Route("[controller]")]
public class PizzaController : ControllerBase
{
    public PizzaController()
    {
    }

    // GET all action
    [HttpGet]
    public ActionResult<List<Pizza>> GetAll() =>
    PizzaService.GetAll();

    // GET by Id action
    [HttpGet("{id}")]
    public ActionResult<Pizza> Get(int id)
    {
        var pizza = PizzaService.Get(id);

        if (pizza == null)
            return NotFound();

        return pizza;
    }

    // POST action
    [HttpPost]
    public IActionResult Create(Pizza pizza)
    {
        PizzaService.Add(pizza);
        return CreatedAtAction(nameof(Get), new { id = pizza.Id }, pizza);
    }

    // PUT action
    [HttpPut("{id}")]
    public IActionResult Update(int id, Pizza pizza)
    {
        if (id != pizza.Id)
            return BadRequest();

        var existingPizza = PizzaService.Get(id);
        if (existingPizza is null)
            return NotFound();

        PizzaService.Update(pizza);

        return NoContent();
    }

    // DELETE action
    [HttpDelete("{id}")]
    public IActionResult Delete(int id)
    {
        var pizza = PizzaService.Get(id);

        if (pizza is null)
            return NotFound();

        PizzaService.Delete(id);

        return NoContent();
    }
}

public class Pizza
{
    public int Id { get; set; }
    public string? Name { get; set; }
    public bool IsGlutenFree { get; set; }
}
