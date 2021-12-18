# rules_go reproduction

The `pure` attribute for the `go_binary` rule does not have an effect when
`goarch` and `goos` are not configured.

```sh
❯ bazel query 'kind(go_transition_binary, //:*)' --output label_kind
go_transition_binary rule //:pure_linux_amd64
go_transition_binary rule //:pure_darwin_amd64
go_transition_binary rule //:pure_auto_auto
go_transition_binary rule //:impure_linux_amd64
go_transition_binary rule //:impure_darwin_amd64
go_transition_binary rule //:impure_auto_auto
```

The `//:pure_auto_auto` target will fail to build, where the corresponding
targets which have `goarch` and `goos` configured build successfully, as
expected. The same behaviour can be observed when building the
`//:pure_auto_auto` with the platforms exposed by rules_go.

all:

| command                           | expected | actual |
| --------------------------------- | -------- | ------ |
| `bazel build //:impure_auto_auto` | fail     | fail   |
| `bazel build //:pure_auto_auto`   | ok       | fail   |

darwin x86_64:

| command                             | expected | actual |
| ----------------------------------- | -------- | ------ |
| `bazel build //:impure_linux_amd64` | fail     | fail   |
| `bazel build //:pure_linux_amd64`   | ok       | ok     |

linux x86_64:
| command                              | expected | actual |
| ------------------------------------ | -------- | ------ |
| `bazel build //:impure_darwin_amd64` | fail     | fail   |
| `bazel build //:pure_darwin_amd64`   | ok       | ok     |
