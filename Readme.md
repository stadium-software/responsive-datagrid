# Responsive DataGrids

Stadium's DataGrids are not responsive out-of-the-box. So, here is a method you can use to make your DataGrids display your rows as a column at a set minimum viewport width. 


https://github.com/stadium-software/responsive-datagrid/assets/2085324/aea3fa2b-880f-40ef-ba89-a44ca710c5fc


<hr>

## Application Setup
1. Check the *Enable Style Sheet* checkbox in the application properties

## Database, Connector and DataGrid

1. Use the instructions from [this repo](https://github.com/stadium-software/samples-database) to setup the database and DataGrid for this sample
2. Assign a class to the DataGrid, like "datagrid-responsive". This class will be used to identify the DataGrid that will become responsive. 

## Global Script

1. Create a Global Script and name it "ResponsiveDataGrid"
2. Add an Input parameter to the "ResponsiveDataGrid" script and call it "DataGridClass"
3. Drag a Javascript action into the script and paste the Javascript below into the *code* property
```
var tableClassName = "." + ~.Parameters.Input.DataGridClass;
attachResponsiveClass();

function convertRows() {
    let tblheadings = document.querySelectorAll(".stadium-responsive-datagrid thead tr th");
    let tblrows = document.querySelectorAll(".stadium-responsive-datagrid tbody tr");
    for (let i = 0; i < tblrows.length; i++) {
        let tblcells = tblrows[i].querySelectorAll("td");
        for (let th = 0; th < tblheadings.length; th++) {
            tblcells[th].setAttribute("name", tblheadings[th].innerText);
        }
    }
}
function attachResponsiveClass() { 
    document.querySelector(tableClassName).classList.add("stadium-responsive-datagrid");
}
var el = document.querySelector(".stadium-responsive-datagrid .table"),
    options = {
        characterData: true,
        attributes: false,
        childList: true,
        subtree: true,
        characterDataOldValue: true,
    },
    observer = new MutationObserver(convertRows);
observer.observe(el, options);
```

## Page.Load Event Handler

1. Drag the "ResponsiveDataGrid" global script into the event handler as the **first action** in the script (important!)
2. Enter the class you added to the DataGrid above into the *DataGridClass* input parameter (e.g. "datagrid-responsive")

## Customising the ResponsiveDataGrid
The *responsive-datagrid-variables.css* file included in this repo contains a set of variables that can be changed to customise the responsive view
1. Open the CSS file called [*responsive-datagrid-variables.css*](responsive-datagrid-variables.css) from this repo in an editor of your choice (I recommend [VS Code](https://code.visualstudio.com/))
2. Adjust the variables in the *:root* element as you see fit

## Applying the CSS
How to apply the CSS to your application
1. Create a folder called *CSS* inside of your Embedded Files in your application
2. Drag the two CSS files from this repo [*responsive-datagrid-variables.css*](responsive-datagrid-variables.css) and [*responsive-datagrid.css*](responsive-datagrid.css) into that folder

#### Stadium 6 (versions 6.6 and above)
1. Paste the link tags below into the *head* property of your application
```
<link rel="stylesheet" href="{EmbeddedFiles}/CSS/responsive-datagrid.css">
<link rel="stylesheet" href="{EmbeddedFiles}/CSS/responsive-datagrid-variables.css">
``` 

#### Stadium 5
1. Add a Javascript action into the Page.load event handler 
2. Paste the Javascript below into the Javascript action Code Editor popup
```
let URL = window.location.protocol + "//" + window.location.host + "/" + window.location.pathname + "//";
let el1 = document.createElement("link");
el1.setAttribute("rel","stylesheet");
el1.setAttribute("href",URL + "Content/EmbeddedFiles/CSS/responsive-datagrid.css");
document.querySelector("head").appendChild(el1);
let el2 = document.createElement("link");
el2.setAttribute("rel","stylesheet");
el2.setAttribute("href",URL + "Content/EmbeddedFiles/CSS/responsive-datagrid-variables.css");
document.querySelector("head").appendChild(el2);
``` 

## Upgrading
To upgrade this module
1. Pull the latest repo
2. If you have made changes to the *responsive-datagrid-variables.css* file in your local repo, merge them
3. You can drag the *responsive-datagrid.css* file into the EmbeddedFiles folder of your application as is
4. Select "Overwrite" when prompted in Stadium
5. Open the *responsive-datagrid-variables.css* file 
6. If new variables were added, change the variables as you see fit 
7. Drag the updated *responsive-datagrid-variables.css* file into the EmbeddedFiles folder of your application
