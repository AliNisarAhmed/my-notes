## Problems that I ran into when testing Ecofitt

1. Input and label do not have "htmlFor" and "id"s.
2. Even if you add both above, input does not get the id, in fact, antd renders a span with that `id`, which has `input` inside of it.
3. So, I had to give the `input` a `data-testId` attibute to test
4. `DropDownInputAnt` does not pass down the `id` prop to the children!.
5. `TextInput` is accessbile though, we can link `htmlFor` with an `id` on `TextInput`
