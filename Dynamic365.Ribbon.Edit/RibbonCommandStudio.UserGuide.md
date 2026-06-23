# Ribbon Command Studio

Ribbon Command Studio is an XrmToolBox plugin for safely inspecting, editing, validating, previewing, and exporting Dataverse and Dynamics 365 ribbon command XML.

The tool is designed for makers and developers who need direct control over `RibbonDiffXml` without manually editing an entire solution package by hand. It provides an XML-first workflow with button preview, validation, formatting, and solution export support.

## What It Does

- Imports Dataverse/Dynamics 365 solution `.zip` files.
- Reads `customizations.xml` and finds entity `RibbonDiffXml`.
- Displays ribbon buttons by area, including form, homepage grid, subgrid, and application ribbon sections.
- Lets you add, edit, delete, and reorder custom ribbon buttons.
- Lets you edit full `RibbonDiffXml` directly.
- Provides XML formatting and validation before applying changes.
- Helps search JavaScript web resources and function names for command actions and rules.
- Exports an updated solution package with formatted XML.
- Can increment the solution version during export.

## When To Use It

Use Ribbon Command Studio when you want to:

- Add a custom button to a model-driven app form or grid.
- Connect a ribbon button to a JavaScript action.
- Add display rules or enable rules to control button visibility or availability.
- Review the generated `RibbonDiffXml` before importing a solution back into Dataverse.
- Fix or compare ribbon XML in an exported solution package.
- Make controlled XML changes without relying only on visual ribbon designers.

## Basic Workflow

1. Open XrmToolBox and connect to your Dataverse environment.
2. Open **Ribbon Command Studio**.
3. Click **Import Solution** and select an exported solution `.zip` file.
4. Review the ribbon preview areas.
5. Select the target ribbon area or an existing button.
6. Add, edit, delete, or reorder custom buttons as needed.
7. Use the XML editor to inspect or adjust the generated `RibbonDiffXml`.
8. Click **Validate XML** before applying changes.
9. Click **Apply XML** to update the in-memory solution document.
10. Click **Export Solution** to create an updated solution package.
11. Import the exported package into Dataverse and publish customizations.

## Ribbon Areas

Ribbon button location is controlled by the `CustomAction` `Location` value in `RibbonDiffXml`.

Common areas include:

- **Form**: buttons shown on a record form command bar.
- **Homepage Grid**: buttons shown on the main table grid.
- **Subgrid**: buttons shown on a grid embedded inside a form.
- **Application**: buttons shown in the application-level ribbon area.

The tool groups buttons by these areas so you can understand where a button will be inserted.

## Button XML Basics

A custom button usually needs these parts:

- `CustomAction`: decides where the button appears and its sequence.
- `Button`: defines the button id, label, tooltip, command, and template alias.
- `CommandDefinition`: defines what the button does.
- `Actions`: contains JavaScript actions or other command actions.
- `DisplayRules`: controls whether the button is visible.
- `EnableRules`: controls whether the button is enabled.
- `LocLabels`: stores localized label and tooltip text.

## JavaScript Actions

A JavaScript button action usually looks like this:

```xml
<JavaScriptFunction Library="$webresource:lvo_/scripts/orderdeskrequest_form.js" FunctionName="Namespace.FunctionName">
  <CrmParameter Value="PrimaryControl" />
</JavaScriptFunction>
```

Use the JavaScript search feature to find available web resources and function names, then insert the full function name into the command XML.

## Display Rules And Enable Rules

Display rules control visibility. Enable rules control whether the button is clickable.

Multiple rules listed under the same command are treated as an AND-style condition by the ribbon framework: all referenced rules must pass for the button to show or enable.

Example command rule references:

```xml
<CommandDefinition Id="sample.Command">
  <EnableRules>
    <EnableRule Id="sample.Status.EnableRule" />
    <EnableRule Id="sample.CustomJs.EnableRule" />
  </EnableRules>
  <DisplayRules>
    <DisplayRule Id="sample.Security.DisplayRule" />
  </DisplayRules>
  <Actions>
    <JavaScriptFunction Library="$webresource:sample_/scripts/form.js" FunctionName="Sample.Ribbon.onClick">
      <CrmParameter Value="PrimaryControl" />
    </JavaScriptFunction>
  </Actions>
</CommandDefinition>
```

## Validation Recommendations

Before exporting:

- Use **Validate XML** after manual XML edits.
- Confirm every `Button` references an existing `CommandDefinition`.
- Confirm every command rule reference exists in `RuleDefinitions`.
- Confirm JavaScript `Library` values use the expected `$webresource:` format.
- Confirm `FunctionName` contains the full namespace and function name when required.
- Confirm `Sequence` values are unique enough to avoid unexpected ordering.
- Confirm label references exist in `LocLabels`.

## Export And Import

After exporting the updated solution package:

1. Import the exported `.zip` into Dataverse.
2. Publish all customizations.
3. Refresh the model-driven app.
4. If a button does not appear, clear browser cache or test in an InPrivate window.
5. Check display rules, enable rules, command actions, and location XML.

## Important Notes

- Built-in ribbon buttons are shown for context but should not be edited as custom button XML.
- Only custom ribbon XML should be modified.
- Always keep a backup of the original exported solution package.
- Test changes in a development or test environment before importing into production.
- If you manually edit XML, validate before applying and exporting.

## Support

If you find an issue, include:

- XrmToolBox version.
- Ribbon Command Studio version.
- Dataverse environment type.
- The solution package version.
- The relevant `RibbonDiffXml` snippet.
- A screenshot or description of the expected and actual behavior.
