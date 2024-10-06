`cmd` + `shift` + `p` : VSC 명령 팔레트 단축키
`Preferences: Open Keyboard Shortcuts (JSON)` 검색
 ![[스크린샷 2024-10-06 오후 1.20.08.png]]
아래 코드를 복붙하면 mac에서 `option`키와  방향키로 camelcase 단위로 이동이 가능해진다.
``` json
// Place your key bindings in this file to override the defaults
[
  // CamelCase cursor
  {
    "key": "alt+right",
    "command": "cursorWordPartRight",
    "when": "editorTextFocus"
  },
  {
    "key": "alt+left",
    "command": "cursorWordPartLeft",
    "when": "editorTextFocus"
  },
  {
    "key": "alt+shift+right",
    "command": "cursorWordPartRightSelect",
    "when": "editorTextFocus"
  },
  {
    "key": "alt+shift+left",
    "command": "cursorWordPartLeftSelect",
    "when": "editorTextFocus"
  }
]
```

