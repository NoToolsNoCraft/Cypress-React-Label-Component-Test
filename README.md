# Cypress React Label Component Test

This example provides a React LogoutButton component integrated with Auth0 for user logout functionality. A Cypress test ensures the button renders and triggers logout correctly. The button redirects users to the application's origin upon logout.


## React Label Component



```bash
import { Text } from "@radix-ui/themes";
import useLanguage from "../hooks/useLanguage";

type LabelProps = {
  labelId: string;
  getLabel?: (id: string) => string;
};

const Label = ({ labelId, getLabel }: LabelProps) => {
  const language = useLanguage();
  const labelGetter = getLabel ?? language.getLabel;

  return <Text>{labelGetter(labelId)}</Text>;
};

export default Label;
```


## Cypress Component Test



```bash
import Label from "../../src/components/Label";

const getLabel = (id: string) => {
  const labels: Record<string, string> = {
    greeting: "Hello World",
    farewell: "Goodbye",
    empty: "",
  };
  return labels[id] ?? "Unknown label";
};

describe("Label component (Cypress)", () => {
  it("renders the correct label for a known labelId", () => {
    cy.mount(<Label labelId="greeting" getLabel={getLabel} />);
    cy.contains("Hello World").should("exist");
  });

  it("renders the correct label for another known labelId", () => {
    cy.mount(<Label labelId="farewell" getLabel={getLabel} />);
    cy.contains("Goodbye").should("exist");
  });

  it("renders empty text for an empty label", () => {
    cy.mount(<Label labelId="empty" getLabel={getLabel} />);
    cy.get("span, span[data-radix-ui-text]").should("be.empty");
  });

  it("renders 'Unknown label' for an unknown labelId", () => {
    cy.mount(<Label labelId="unknown" getLabel={getLabel} />);
    cy.contains("Unknown label").should("exist");
  });
});
```

![Screenshot of Label Component](Screenshot%202025-09-27%20123846.png)

| Criteria                  | Justification                                                                                                                                                                                              |
| :------------------------ | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Isolation**             | The test isolates the `Label` component by **injecting a custom `getLabel` function**. This removes dependency on external logic and ensures the test verifies only the component’s rendering behavior.    |
| **Mocking Quality**       | Uses a **controlled stub function (`getLabel`)** to simulate label retrieval, enabling predictable results for known, empty, and unknown cases.                                                            |
| **Coverage**              | Covers all expected label rendering scenarios: <br>1) Known labels (`greeting`, `farewell`) <br>2) Empty label case (`empty`) <br>3) Unknown label case (`unknown`). Ensures complete behavioral coverage. |
| **Readability**           | Tests are clearly structured with descriptive names for both test cases and mock data, making them easy to understand and maintain.                                                                        |
| **Clarity of Assertions** | Assertions clearly check the expected text or empty content in each case, avoiding over-testing and ensuring each case is directly tied to the intended behavior.                                          |
                                                             
