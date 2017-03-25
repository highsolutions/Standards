## Main rules of code format

* UTF-8 (without BEM)
* Indentation
    * We're using tabs, not spaces
    * Tab size: 4 spaces
    * Tabs are tabs, no replacement into spaces

## Editors


### .editorconfig

    # This file is for unifying the coding style for different editors and IDEs
    # editorconfig.org
    root = true

    [*]
    end_of_line = lf
    charset = utf-8
    indent_style = tab
    insert_final_newline = true
    trim_trailing_whitespace = true

    [*.bat]
    end_of_line = crlf

    [*.yml]
    indent_style = space
    indent_size = 2

### Sublime Text

    // The number of spaces a tab is considered equal to
    "tab_size": 4,

    // Set to true to insert spaces when tab is pressed
    "translate_tabs_to_spaces": false,

    // Determines what character(s) are used to terminate each line in new files.
    // Valid values are 'system' (whatever the OS uses), 'windows' (CRLF) and
    // 'unix' (LF only).
    "default_line_ending": "unix",

    // Trims white space added by auto_indent when moving the caret off the
    // line.
    "trim_automatic_white_space": true,

### NetBeans

    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE editor-preferences PUBLIC "-//NetBeans//DTD Editor Preferences 1.0//EN" "http://www.netbeans.org/dtds/EditorPreferences-1_0.dtd">
    <editor-preferences>
        <entry javaType="java.lang.Boolean" name="expand-tabs" xml:space="preserve">
            <value><![CDATA[false]]></value>
        </entry>
        <entry javaType="java.lang.Integer" name="tab-size" xml:space="preserve">
            <value><![CDATA[4]]></value>
        </entry>
    </editor-preferences>

### PHPStorm

    <code_scheme name="Laravel">
      <option name="OTHER_INDENT_OPTIONS">
        <value>
          <option name="INDENT_SIZE" value="4" />
          <option name="CONTINUATION_INDENT_SIZE" value="8" />
          <option name="TAB_SIZE" value="4" />
          <option name="USE_TAB_CHARACTER" value="true" />
          <option name="SMART_TABS" value="false" />
          <option name="LABEL_INDENT_SIZE" value="0" />
          <option name="LABEL_INDENT_ABSOLUTE" value="false" />
          <option name="USE_RELATIVE_INDENTS" value="false" />
        </value>
      </option>
      <PHPCodeStyleSettings>
        <option name="ALIGN_KEY_VALUE_PAIRS" value="true" />
        <option name="ALIGN_PHPDOC_PARAM_NAMES" value="true" />
        <option name="ALIGN_PHPDOC_COMMENTS" value="true" />
        <option name="CONCAT_SPACES" value="false" />
        <option name="PHPDOC_BLANK_LINES_AROUND_PARAMETERS" value="true" />
        <option name="PHPDOC_WRAP_LONG_LINES" value="true" />
        <option name="LOWER_CASE_BOOLEAN_CONST" value="true" />
        <option name="LOWER_CASE_NULL_CONST" value="true" />
        <option name="BLANK_LINE_BEFORE_RETURN_STATEMENT" value="true" />
        <option name="KEEP_RPAREN_AND_LBRACE_ON_ONE_LINE" value="true" />
        <option name="SPACE_BEFORE_UNARY_NOT" value="true" />
        <option name="SPACE_AFTER_UNARY_NOT" value="true" />
        <option name="SPACES_WITHIN_SHORT_ECHO_TAGS" value="false" />
        <option name="FORCE_SHORT_DECLARATION_ARRAY_STYLE" value="true" />
      </PHPCodeStyleSettings>
      <XML>
        <option name="XML_LEGACY_SETTINGS_IMPORTED" value="true" />
      </XML>
      <codeStyleSettings language="PHP">
        <option name="LINE_COMMENT_AT_FIRST_COLUMN" value="false" />
        <option name="KEEP_LINE_BREAKS" value="false" />
        <option name="KEEP_BLANK_LINES_IN_DECLARATIONS" value="1" />
        <option name="KEEP_BLANK_LINES_IN_CODE" value="1" />
        <option name="BLANK_LINES_AFTER_PACKAGE" value="1" />
        <option name="BLANK_LINES_AROUND_FIELD" value="1" />
        <option name="BLANK_LINES_AROUND_METHOD" value="2" />
        <option name="BLANK_LINES_AFTER_CLASS_HEADER" value="1" />
        <option name="FINALLY_ON_NEW_LINE" value="true" />
        <option name="ALIGN_MULTILINE_PARAMETERS" value="false" />
        <option name="ALIGN_MULTILINE_EXTENDS_LIST" value="true" />
        <option name="SPACE_WITHIN_ARRAY_INITIALIZER_BRACES" value="true" />
        <option name="SPACE_AFTER_TYPE_CAST" value="true" />
        <option name="CALL_PARAMETERS_WRAP" value="1" />
        <option name="METHOD_PARAMETERS_WRAP" value="5" />
        <option name="METHOD_PARAMETERS_LPAREN_ON_NEXT_LINE" value="true" />
        <option name="METHOD_PARAMETERS_RPAREN_ON_NEXT_LINE" value="true" />
        <option name="ARRAY_INITIALIZER_WRAP" value="5" />
        <option name="ARRAY_INITIALIZER_LBRACE_ON_NEXT_LINE" value="true" />
        <option name="ARRAY_INITIALIZER_RBRACE_ON_NEXT_LINE" value="true" />
        <option name="IF_BRACE_FORCE" value="3" />
        <option name="DOWHILE_BRACE_FORCE" value="3" />
        <option name="WHILE_BRACE_FORCE" value="3" />
        <option name="FOR_BRACE_FORCE" value="3" />
      </codeStyleSettings>
    </code_scheme>

### VSCode

    // Controls the rendering size of tabs in characters.
    // If set to auto, the value will be guessed based on the opened file.
    "editor.tabSize": 4,

    // Controls if the editor will insert spaces for tabs.
    // If set to auto, the value will be guessed based on the opened file.
    "editor.insertSpaces": false
