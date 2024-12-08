NOS Product Report  

### 1\. Creating the Route in Next.js

#### File: pages/report/nos-product-report.js

Code Explanation:

javascript

Copy code

import NosProductReportComponent from "../../general/components/modules/dashboard/report/nos-product-report-component";

import { Constant } from "../../general/helpers/constant";

import PageHOC from "../../general/components/shared/page-hoc";

const NosProductReport = ({ showSingleButtonPopup, showLoader }) => {

return (

<NosProductReportComponent

showSingleButtonPopup={showSingleButtonPopup}

showLoader={showLoader}

/>

);

};

export default PageHOC(

NosProductReport,

Constant.MENU_SEQUENCE_NUMBER.nosProductReport,

"Nos Product Report"

);

Purpose and Flow:

1. This file defines the route /report/nos-product-report in the Next.js application.
2. It imports:
    - NosProductReportComponent: The UI logic and layout for the report.
    - Constant: Holds constants used throughout the project (like menu sequence numbers).
    - PageHOC: A Higher-Order Component (HOC) that wraps the report page, adding functionality like layout, permissions, or additional props.

Connection:

- NosProductReport acts as the entry point for the feature and delegates rendering to NosProductReportComponent.
- PageHOC enhances the page with extra features like dynamic page titles.

2\. Creating the UI Component

#### File: general/components/modules/dashboard/report/nos-product-report-component.js

Code Explanation:

javascript

Copy code

import { Constant } from "../../../../helpers/constant";

import styles from "../../../../../styles/commonCss.module.css";

import TableComponent from "../../../shared/table";

import { ReportConstant } from "./report.constant";

import FormSelect from "../../../shared/form/select";

import FormButton from "../../../shared/form/button";

import { useState } from "react";

import FormInputWithLabel from "../../../shared/form/input-with-label";

import ExportExcelPdfComponent from "../../../shared/export-excel-pdf-component";

const NosProductReportComponent = ({ showLoader, showSingleButtonPopup }) => {

const { nosProductReport } = ReportConstant;

const \[screenSize, setScreenSize\] = useState(1000);

const \[tableRows, setTableRows\] = useState(\[\]);

const \[tableResponse, setTableResponse\] = useState(\[\]);

const \[defaultCategoryOptions, setDefaultCategoryOptions\] = useState(\[\]);

const \[formValues, setFormValues\] = useState({

defaultCategory: "",

});

const dropdownDefaultCategoryOptions = \[

{ value: "all", label: "All" },

...defaultCategoryOptions,

\];

const onFormValueChange = (e) => {

const { name, value } = e.target;

setFormValues((prev) => ({

...prev,

\[name\]: value,

}));

};

const onSearchButtonClick = () => {};

const onClearButtonClick = () => {};

return (

&lt;div className={styles.ml48}&gt;

<div

className={styles.rightSectionwrapper}

style={{

border: "1px solid" + Constant.CATEGORY_INITIAL_COLOUR_CODE.report,

}}

\>

{screenSize > Constant.THRESHOLD_TO_SHOW_MOBILE_CONTENT && (

&lt;div className={styles.tabMessageWrapper}&gt;

&lt;span className={styles.tabMessage + " " + styles.top0}&gt;

{nosProductReport.heading}

&lt;/span&gt;

&lt;/div&gt;

)}

&lt;div className={styles.searchWrapper + " " + styles.mt10}&gt;

&lt;div className={styles.textFieldWrapper}&gt;

&lt;div className={styles.textFieldNameWrapper}&gt;

<FormSelect

label={nosProductReport.defaultCategoryLabel}

name="defaultCategory"

value={formValues.defaultCategory}

onChange={onFormValueChange}

options={dropdownDefaultCategoryOptions}

/>

&lt;/div&gt;

&lt;div className={styles.textFieldNameWrapper}&gt;

<FormInputWithLabel

label={nosProductReport.productNameLabel}

value={formValues.defaultCategory}

name="Product Name"

/>

&lt;/div&gt;

&lt;/div&gt;

&lt;div className={styles.searchAndDownloadButtonWrapper}&gt;

&lt;div&gt;&lt;/div&gt;

&lt;div className={styles.ButtonWrapper}&gt;

<FormButton

label={nosProductReport.searchButtonLabel}

class="primary-button"

onClick={onSearchButtonClick}

/>

<FormButton

label={nosProductReport.clearButtonLabel}

class="secondary-button"

onClick={onClearButtonClick}

/>

&lt;/div&gt;

<ExportExcelPdfComponent

isExcel={true}

showSingleButtonPopup={showSingleButtonPopup}

headers={nosProductReport.tableHeaders}

rows={tableRows}

takeFirstColumn={true}

buttonClass="primary-button"

label={Constant.COMMON.exportToExcelButton}

filename={Constant.EXCEL_AND_PDF_FILE_NAMES.customerPayment}

/>

<ExportExcelPdfComponent

isExcel={false}

showSingleButtonPopup={showSingleButtonPopup}

headers={nosProductReport.tableHeaders}

rows={tableRows}

takeFirstColumn={true}

buttonClass="secondary-button"

label={Constant.COMMON.exportToPdfButton}

filename={Constant.EXCEL_AND_PDF_FILE_NAMES.customerPayment}

/>

&lt;div&gt;&lt;/div&gt;

&lt;/div&gt;

<TableComponent

id="nosProductReportId"

dataRows={tableRows}

tableHeader={nosProductReport.tableHeaders}

emptyMessage={Constant.EMPTY_TABLE_MESSAGE.enterSearchCriteria}

/>

&lt;/div&gt;

&lt;/div&gt;

&lt;/div&gt;

);

};

export default NosProductReportComponent;

Purpose and Flow:

1. The file defines the structure of the report, including:
    - Filters: Dropdown for categories and an input field for the product name.
    - Action Buttons: Search, Clear, and Export (to Excel or PDF).
    - Table: Displays the report data.
2. State Management:
    - formValues: Tracks form inputs.
    - tableRows: Holds the data rows to be shown in the table.

Connection:

- This is the main UI file connected to nos-product-report.js.
- User actions (e.g., clicking Search) will interact with state and potentially trigger API calls (not implemented here).

### 3\. Defining Constants

#### File: report.constant.js

javascript

Copy code

static nosProductReport = {

heading: "Nos Product Report",

defaultCategoryLabel: "Default Category",

productNameLabel: "Product Name",

searchButtonLabel: "Search",

clearButtonLabel: "Clear",

tableHeaders: \[

{ fieldValue: "Product Name", aligned: "-webkit-center" },

{ fieldValue: "Size", aligned: "-webkit-center" },

{ fieldValue: "Color", aligned: "-webkit-center" },

{ fieldValue: "MRP", aligned: "-webkit-center" },

{ fieldValue: "Selling Price", aligned: "-webkit-center" },

{ fieldValue: "Cost", aligned: "-webkit-center" },

{ fieldValue: "Cost Price", aligned: "-webkit-center" },

{ fieldValue: "Buy To Sell Multiplier", aligned: "-webkit-center" },

\],

};

Purpose and Flow:

- Centralizes text and labels used in the UI.
- Connection: Used in NosProductReportComponent to populate headings, labels, and table headers.

### 4\. Menu Integration

#### File: common-services.js

javascript

Copy code

if (checkHasSubMenu(menuList, Constant.MENU_SEQUENCE_NUMBER.nosProductReport)) {

reportSubMenuList.push({

name: "Nos Product Report",

link: "/report/nos-product-report",

image: fromSidebar ? orderReportSideBarIcon : orderReportIcon,

hoverImage: fromSidebar ? orderReportSideBarIcon : orderReportHoverIcon,

});

}

return reportSubMenuList;

Purpose and Flow:

1. Adds the Nos Product Report option to the sidebar menu.
2. Connection:
    - Checks permissions (via checkHasSubMenu) before adding the menu item.

### Overall Flow

1. User navigates to /report/nos-product-report.
2. pages/report/nos-product-report.js renders NosProductReportComponent.
3. UI is displayed, with filters, buttons, and a table.
4. Constants provide labels and table configurations.
5. Users can interact with filters, and buttons trigger placeholder functions (to be implemented later).

# Backend: Detailed Explanation for Beginners

The backend is the server-side code that handles data, processes user requests, and sends the appropriate responses. It acts like the brain of the application.

## 1\. Request Class: NosProductReportRequest

This class is used to accept input data from the user when they make a request to the API.

### Code

java

Copy code

package com.tsd.okgenieapi.report.nos_product_report.request;

import javax.validation.constraints.NotBlank;

import lombok.AllArgsConstructor;

import lombok.Data;

import lombok.NoArgsConstructor;

@Data

@NoArgsConstructor

@AllArgsConstructor

public class NosProductReportRequest {

@NotBlank(message = "Outlet Id is mandetory")

private String outletId;

private String defaultCategoryId;

private String outletProductId;

private int pageNumber;

private int pageSize;

}

### What Does This File Do?

1. Purpose: This file holds the data that the user sends to the API. For example, if the user wants a report of products, they may send:
    - The outletId (to specify which store's data to fetch).
    - Filters like defaultCategoryId (to show products only from a specific category).
    - Pagination parameters like pageNumber and pageSize (to divide the results into pages).

### Breakdown of the Code

1. @Data Annotation:
    - This is part of the Lombok library.
    - Automatically generates common methods like getters, setters, and toString, so you don’t need to write them manually.
2. @NoArgsConstructor and @AllArgsConstructor:
    - @NoArgsConstructor: Creates an empty/default constructor (used when no values are passed).
    - @AllArgsConstructor: Creates a constructor with all fields initialized.
3. Fields:
    - outletId: The ID of the outlet (mandatory).
    - defaultCategoryId: Optional field to filter by product category.
    - outletProductId: Optional field to fetch a specific product.
    - pageNumber and pageSize: Define which page of data to fetch and how many items per page.
4. @NotBlank:
    - Ensures that outletId cannot be left blank or null.
    - If outletId is missing, the system will return the error: "Outlet Id is mandatory".

## 2\. Response Class: NosProductReportResponse

This class defines how the data will be sent back to the user.

### Code

java

Copy code

package com.tsd.okgenieapi.report.nos_product_report.response;

import lombok.AllArgsConstructor;

import lombok.Data;

import lombok.NoArgsConstructor;

@Data

@NoArgsConstructor

@AllArgsConstructor

public class NosProductReportResponse {

private String outletProductId;

private String productName;

private String size;

private String color;

private Double mrp;

private Double sellingPrice;

private Double cost;

private Double costPrice;

private Integer buyToSaleUomMultipler;

}

### What Does This File Do?

1. Purpose: This file contains the structure of the data that the API will send back to the frontend.

Example Response:  
json  
Copy code  
{

"outletProductId": "12345",

"productName": "T-Shirt",

"size": "L",

"color": "Blue",

"mrp": 500.0,

"sellingPrice": 450.0,

"cost": 300.0,

"costPrice": 250.0,

"buyToSaleUomMultipler": 1

}

### Field Descriptions

1. outletProductId: The unique ID of the product in the outlet.
2. productName: The name of the product.
3. size: The size of the product (e.g., S, M, L, XL).
4. color: The color of the product (e.g., Blue, Red).
5. mrp: Maximum retail price of the product.
6. sellingPrice: The price at which the product is sold.
7. cost: The actual cost of the product for the seller.
8. costPrice: Another variant of cost (used for detailed financial analysis).
9. buyToSaleUomMultipler: A multiplier for unit conversions (e.g., packs of 6).

## 3\. Service Layer

The service layer contains the business logic for processing data.

### Service Interface: NosProductReportService

This is like a contract that defines what actions the service should perform.

### Code

java

Copy code

public interface NosProductReportService {

GenericResponse&lt;NosProductReportResponse&gt; searchNosProductReport(NosProductReportRequest request);

}

### What Does This File Do?

- Declares a method searchNosProductReport, which will:
    1. Accept the NosProductReportRequest object as input.
    2. Return a GenericResponse containing the NosProductReportResponse.

### Service Implementation: NosProductReportServiceImpl

This is the class where the actual business logic is written.

### Code

java

Copy code

@Service

public class NosProductReportServiceImpl implements NosProductReportService {

@Autowired

private SearchServiceImpl&lt;OutletProductEntity, NosProductReportResponse&gt; serviceImpl;

@Autowired

private OKGenieUtil okGenieUtil;

@Override

@Transactional(readOnly = true)

public GenericResponse&lt;NosProductReportResponse&gt; searchNosProductReport(NosProductReportRequest request) {

Pageable pageable = okGenieUtil.getPageable(request.getPageNumber(), request.getPageSize(), "name");

Page&lt;NosProductReportResponse&gt; nosProductReportResponseList = serviceImpl.searchNosProductReport(request,

pageable, OutletProductEntity.class, NosProductReportResponse.class);

return new GenericResponse&lt;NosProductReportResponse&gt;(ResponseCode.SUCCESS.getStatusCode(), "Success",

nosProductReportResponseList.getNumberOfElements(), nosProductReportResponseList.getTotalPages(),

nosProductReportResponseList.getTotalElements(), nosProductReportResponseList.getContent());

}

}

### What Does This File Do?

1. Class-level Annotations:
    - @Service: Marks this class as a service component. Spring will automatically manage this class.
2. @Transactional(readOnly = true):
    - Ensures that database transactions are read-only for this method, improving performance.
3. Steps in the Method:
    - okGenieUtil.getPageable: Creates pagination parameters (page number, size, and sort order).
    - serviceImpl.searchNosProductReport: Calls a helper method to perform the database query.
    - GenericResponse: Wraps the result in a response format to send back to the frontend.

<br/>Purpose of the Method

The method searchNosProductReport is designed to query a database and retrieve a list of products that match specific search criteria. It supports pagination, so only a portion of the results is returned at a time, based on the requested page number and size.

### Method Signature

java

Copy code

public Page&lt;R&gt; searchNosProductReport(NosProductReportRequest request, Pageable pageable, Class&lt;T&gt; classname, Class&lt;R&gt; responseClass)

1. Inputs:
    - NosProductReportRequest request: Contains the search filters like outlet ID, category ID, and product ID.
    - Pageable pageable: Contains pagination information (page number, page size, and sorting details).
    - Class&lt;T&gt; classname: Represents the database entity class being queried.
    - Class&lt;R&gt; responseClass: Represents the class used to structure the query results.
2. Output:
    - A Page&lt;R&gt; object containing the filtered, paginated list of products.

### Step-by-Step Explanation

#### 1\. Call a Common Method

java

Copy code

commonMethod(classname, responseClass);

- Purpose: This likely sets up some shared configurations or validations for the query.
- Input: The classname and responseClass parameters.
- Output: Not directly used here but might configure the query.

#### 2\. Initialize Lists

java

Copy code

List&lt;R&gt; outletProductList = new ArrayList<>();

List&lt;Predicate&gt; searchCriteria = new ArrayList<>();

- outletProductList: Stores the results of the query.
- searchCriteria: Stores conditions (filters) for querying the database.

#### 3\. Build the Query Columns

java

Copy code

cq.select(cb.construct(responseClass,

iRoot.get(iRoot.getModel().getDeclaredId(String.class)),

iRoot.get("name"),

iRoot.get("size"),

iRoot.get("color"),

iRoot.get("MRP"),

iRoot.get("sellingPrice"),

iRoot.get("cost"),

iRoot.get("costPrice"),

iRoot.get("buyToSaleUomMultipler")));

- What it does:
  - Specifies which columns of the database should be included in the results.
  - Uses cb.construct to map database columns to fields in the responseClass.
  - Includes fields like name, size, color, and prices.

#### 4\. Add Filters

java

Copy code

searchCriteria.add(cb.equal(iRoot.get("outletId"), request.getOutletId()));

- Filter: Matches the outletId from the database with the outletId provided in the request.

java

Copy code

if (request.getDefaultCategoryId() != null && !request.getDefaultCategoryId().isBlank()) {

searchCriteria.add(cb.equal(iRoot.get("defaultCatgoryId").get("id"), request.getDefaultCategoryId()));

}

- Optional Filter: Adds a filter for defaultCategoryId if it’s provided in the request.

java

Copy code

if (request.getOutletProductId() != null && !request.getOutletProductId().isBlank()) {

searchCriteria.add(cb.equal(iRoot.get("id"), request.getOutletProductId()));

}

- Optional Filter: Adds a filter for outletProductId if it’s provided.

java

Copy code

searchCriteria.add(cb.equal(iRoot.get("nos"), true));

- Final Filter: Ensures that only products marked with nos = true are included.

#### 5\. Convert Filters to Predicates

java

Copy code

Predicate\[\] predicateArray = new Predicate\[searchCriteria.size()\];

searchCriteria.toArray(predicateArray);

cq.where(predicateArray);

- Converts the list of filters into an array of Predicate objects.
- Adds these conditions to the query using cq.where.

#### 6\. Sort the Results

java

Copy code

List&lt;Order&gt; orderList = new ArrayList<>();

orderList.add(cb.asc(iRoot.get("name")));

cq.orderBy(orderList);

- Purpose: Sorts the results by the name field in ascending order.

#### 7\. Execute the Query

java

Copy code

outletProductList = em.createQuery(cq)

.setFirstResult((int) pageable.getOffset())

.setMaxResults(pageable.getPageSize())

.getResultList();

- How it works:
  - createQuery(cq): Executes the query.
  - setFirstResult: Specifies the starting point (offset) for the results, based on the page number.
  - setMaxResults: Limits the number of results to the page size.

#### 8\. Get Total Results

java

Copy code

Integer size = em.createQuery(cq).getResultList().size();

- Executes the query again to determine the total number of matching records.

#### 9\. Return Paginated Results

java

Copy code

return new PageImpl&lt;R&gt;(outletProductList, pageable, size);

- Purpose: Wraps the query results into a Page object, which includes:
  - The list of products (outletProductList).
  - Pagination information (pageable).
  - Total number of matching records (size).

### How It Works Together

1. Inputs:
    - You pass the search filters (request), pagination details (pageable), and class types.
2. Filters:
    - The method builds a list of conditions (Predicate) based on the provided search parameters.
3. Query:
    - It constructs a query selecting specific fields and applies the filters and sorting.
4. Pagination:
    - Only the relevant page of results is fetched using setFirstResult and setMaxResults.
5. Output:
    - The method returns a Page object, which is ready to be sent to the frontend.

### Beginner Tip

- Predicates: Think of them as "conditions" or "filters" that narrow down what data you want from the database.
- Pageable: Helps you avoid loading too much data at once by splitting it into pages.
- EntityManager (em): A tool for interacting with the database in Java.