# Class Styled-Components

### Steps:

- npx create-react-app name_of_app
- npm install axios styled-components react-toastify
- code .
- npm start
- clean out app.js
- In src folder, create GlobalStyles.js:

  ```
  import { createGlobalStyle } from "styled-components";

    const GlobalStyle = createGlobalStyle`
    body {
    margin: 0;
    padding: 0;
    }

    a {
    text-decoration: none;
    color: inherit;
    }
    `;

    export default GlobalStyle;

  ```

- In index.js, create theme:

```

import {ThemeProvider} from "styled-components";

const theme = {
colors: {
green: "#88b04b",
},
};

```

- Pass the Theme Provider as a wrapper for App component along with global styles:

```

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(
<React.StrictMode>
<ThemeProvider theme={theme}>
<GlobalStyle />
<App />
</ThemeProvider>
</React.StrictMode>
);

```

- In app.js, make call to API, importing axios, useState
- API CALL = `https://en.wikipedia.org/w/api.php?action=query&format=json&origin=*&list=search&srsearch=${query}`
- Response = response.data.query.search
- Use data, query as states
- Create handleSubmit function
- Create Input.jsx Component:

```
import React from "react";
import styled from "styled-components";

const InputTag = styled.input`
  border-radius: 10px;
  height: 20px;
`;

function Input(props) {
  return (
      <InputTag
        type={props.type}
        name=""
        id=""
        value={props.value}
        onChange={(e) => props.updateFunction(e.target.value)}
      />
  );
}

export default Input;
```

- Create Button.jsx component:

```
import React from 'react'
import styled from "styled-components"

const ButtonTag = styled.button`
  background-color: ${({ theme }) => theme.colors.green};
  border-radius: 1rem;
  border-style: none;
  color: #ffffff;
  cursor: pointer;
  height: 40px;
  width: 150px;
  margin: 1rem;
  border-radius: 15px;
  box-shadow: 0.01rem white;
  color: white;
`;

function Button(props) {
  return (
    <ButtonTag>{props.content}</ButtonTag>
  )
}

export default Button
```

- Create Searchform.jsx component:

```
import React from "react";
import styled from "styled-components";
import Button from "./Button";
import Input from "./Input";

const FormTag = styled.form`
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;

`;

function Searchform(props) {
  const { submit, search, updateQuery } = props;
  return (
    <FormTag onSubmit={submit}>
      <Input type={"text"} value={search} updateFunction={updateQuery} />
      <Button type={submit} content={"Search For Your Article!"} />
    </FormTag>
  );
}

export default Searchform;
```

- Import Searchform.jsx in APP.js:

```
<Searchform submit={handleSubmit} search={query} updateQuery={setQuery} />
```

- Create Card.jsx component:

```
import React from "react";
import Button from "./Button";
import styled from "styled-components";

const Cardtag = styled.div`
  margin-left: 50px;
  margin-top: 20px;
  width: 300px;
  box-sizing: 100px;
  display: flex;
  flex-direction: column;
  align-items: center;
  box-shadow: 2px 2px 2px 2px grey;
  border-radius: 15px;

  button {
    margin: 1rem;
    border-radius: 1.5rem;
    width: 10rem;
    height: 3rem;
    border: none;
    box-shadow: 0.01rem white;
    color: white;
  }

  .title {
    text-align: center;
  }
`;

function Card(props) {
  return (
    <Cardtag>
      <h2 className="title">{props.title}</h2>
        <a href={`https://en.wikipedia.org/?curid=${props.link}`} target="_blank" rel="noreferrer">
          <Button content={"Go to Page"} />
        </a>
    </Cardtag>
  );
}

export default Card;
```

- Map through the API response:

```
 {data.length > 0 &&
          data.map((el) => {
            return <Card title={el.title} link={el.pageid} />;
          })}
```

- Create the RowTag to make cards align:

```
const RowTag = styled.div`
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
`;

<RowTag>
        {data.length > 0 &&
          data.map((el) => {
            return <Card title={el.title} link={el.pageid} />;
          })}
      </RowTag>
```

- Create the Randomarticle.jsx component:

```
import React from "react";
import Button from "./Button";

function Randomarticle() {
  return (
    <form action="https://en.wikipedia.org/wiki/Special:Random" target="_blank">
      <Button content={"Get a Random Article from Wikipedia!"} />
    </form>
  );
}

export default Randomarticle;
```

- Import it to App.js on top of the Searchform
- import { ToastContainer, toast } from "react-toastify" && import "react-toastify/dist/ReactToastify.css";
- Create function to display toast:

```
const error = () =>
    toast.error("Please Input your search filter!", {
      position: "top-left",
      autoClose: 3000,
      hideProgressBar: false,
      closeOnClick: true,
      pauseOnHover: false,
    });
```

- Call function on catch((err) => error())
