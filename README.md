# BlazorBrowserStorageExample
Simple demo of using local browser storage in Blazor page.

## Overview

Use the Microsoft.AspNetCore.Components.Server.ProtectedBrowserStorage library to store session data locally in the browser

```C#
@inject ProtectedSessionStorage ProtectedSessionStore  //store is scoped to browser tab.

@inject ProtectedLocalStorage ProtectedLocalStore  //store is scoped to browser window.
```

## Usage

```C#
@page "/counter"

@using Microsoft.AspNetCore.Components.Server.ProtectedBrowserStorage
@inject ProtectedSessionStorage ProtectedSessionStore
@inject ProtectedLocalStorage ProtectedLocalStore


<PageTitle>Counter</PageTitle>

<h1>Counter</h1>

<p role="status">Current count: @currentCountSession  (Session for tab)</p>
<p role="status">Current count: @currentCountLocal (Session for window)</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>

@code {
    /* sessionStorage is scoped to the browser tab. If the user reloads the tab, the state persists. 
        If the user closes the tab or the browser, the state is lost. If the user opens 
        multiple browser tabs, each tab has its own independent version of the data.
    */
    private int currentCountSession = 0;  
    
    /*localStorage is scoped to the browser's window. If the user reloads the page or closes and re-opens 
     the browser, the state persists. If the user opens multiple browser tabs, the state is shared across 
     the tabs. Data persists in localStorage until explicitly cleared.*/
    private int currentCountLocal = 0;

    protected override async Task OnInitializedAsync()
    {
        var result = await ProtectedSessionStore.GetAsync<int>("count");
        currentCountSession = result.Success ? result.Value : 0;

        var resultLocal = await ProtectedLocalStore.GetAsync<int>("countLocal");
        currentCountLocal = resultLocal.Success ? resultLocal.Value : 0;
    }

    private async Task IncrementCount()
    {
        currentCountSession++;
        await ProtectedSessionStore.SetAsync("count", currentCountSession);

        currentCountLocal++;
        await ProtectedLocalStore.SetAsync("countLocal", currentCountLocal);
    }


}
```
