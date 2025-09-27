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

![Screenshot of Label Component](Screenshot%202025-09-27%20142839.png)

| Criteria                  | Justification                                                                                                                                                                                                                   |
| :------------------------ | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Isolation**             | The test isolates the `CancelOrderButton` component without relying on external dependencies, focusing only on the component’s own rendering and interaction logic.                                                             |
| **Mocking Quality**       | No external mocking is required here since the component’s behavior is self-contained; Cypress handles UI interaction directly for predictable and repeatable results.                                                          |
| **Coverage**              | Covers all core interaction scenarios: <br>1) Rendering of the button <br>2) Opening the confirmation dialog <br>3) Closing the dialog via "No" <br>4) Closing the dialog via "Yes". This ensures complete functional coverage. |
| **Readability**           | Tests are clearly structured with descriptive names for each scenario, making the test suite easy to understand, maintain, and extend.                                                                                          |
| **Clarity of Assertions** | Each test asserts the intended visible behavior (button presence, dialog visibility, dialog closure), avoiding over-testing while ensuring the component works as expected under each condition.                                |
