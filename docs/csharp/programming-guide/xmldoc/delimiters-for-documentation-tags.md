---
title: "Delimiters for documentation tags - C# programming guide"
description: Learn about delimiters for documentation tags. Delimiters indicate to the compiler where a documentation comment begins and ends.
ms.date: 07/20/2015
helpviewer_keywords:
  - "XML [C#], delimiters"
  - "/** */ delimiters for C# documentation tags"
  - "/// delimiter for C# documentation"
ms.assetid: 9b2bdd18-4f5c-4c0b-988e-fb992e0d233e
---
# Delimiters for documentation tags (C# programming guide)

The use of XML doc comments requires delimiters, which indicate to the compiler where a documentation comment begins and ends. You can use the following kinds of delimiters with the XML documentation tags:

- `///`

  Single-line delimiter. This is the form that is shown in documentation examples and used by the C# project templates. If there is a white space character following the delimiter, that character is not included in the XML output.

  > [!NOTE]
  > The Visual Studio integrated development environment (IDE) automatically inserts the `<summary>` and `</summary>` tags and moves your cursor within these tags after you type the `///` delimiter in the code editor. You can turn this feature on or off in the [Options dialog box](/visualstudio/ide/reference/options-text-editor-csharp-advanced).
  
- `/** */`

  Multiline delimiters.

  There are some formatting rules to follow when you use the `/** */` delimiters:
  
  - On the line that contains the `/**` delimiter, if the remainder of the line is white space, the line is not processed for comments. If the first character after the `/**` delimiter is white space, that white space character is ignored and the rest of the line is processed. Otherwise, the entire text of the line after the `/**` delimiter is processed as part of the comment.

  - On the line that contains the `*/` delimiter, if there is only white space up to the `*/` delimiter, that line is ignored. Otherwise, the text on the line up to the `*/` delimiter is processed as part of the comment.
  
  - For the lines after the one that begins with the `/**` delimiter, the compiler looks for a common pattern at the beginning of each line. The pattern can consist of optional white space and an asterisk (`*`), followed by more optional white space. If the compiler finds a common pattern at the beginning of each line that does not begin with the `/**` delimiter or the `*/` delimiter, it ignores that pattern for each line.

  The following examples illustrate these rules.

  - The only part of the following comment that's processed is the line that begins with `<summary>`. The three tag formats produce the same comments.

    ```csharp
    /** <summary>text</summary> */

    /**
    <summary>text</summary>
    */

    /**
     * <summary>text</summary>
    */
    ```

  - The compiler identifies a common pattern of " \* " at the beginning of the second and third lines. The pattern is not included in the output.

    ```csharp
    /**
     * <summary>
     * text </summary>*/
    ```

  - The compiler finds no common pattern in the following comment because the second character on the third line is not an asterisk. Therefore, all text on the second and third lines is processed as part of the comment.

    ```csharp
    /**
     * <summary>
       text </summary>
    */
    ```

  - The compiler finds no pattern in the following comment for two reasons. First, the number of spaces before the asterisk is not consistent. Second, the fifth line begins with a tab, which does not match spaces. Therefore, all text from lines two through five is processed as part of the comment.

    <!-- markdownlint-disable MD010 -->
    ```csharp
    /**
      * <summary>
      * text
     *  text2
     	*  </summary>
    */
    ```
    <!-- markdownlint-enable MD010 -->

## See also

- [C# programming guide](../index.md)
- [XML documentation comments](./index.md)
- [-doc (C# compiler options)](../../language-reference/compiler-options/doc-compiler-option.md)
