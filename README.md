# `bigarray-overlap`

This little library allows us to check if to `bigarray` share same memory area:
```
                    ba2
            ba1    .-------.
           .------|.        |
          |       | |       |
0x000800: [-------###-------]
                  | |
		   .
		  ba3
```

In this example, `ba1` and `ba2` share a small area and `bigarray-overlap` can
returns this small area to you as a new `ba3` value.
```ocaml
open Bigarray

let () =
  let ba = Array1.create Char Bigarray.c_layout 17 in
  let ba1 = Array1.sub ba 0 10 in
  let ba2 = Array1.sub ba 7 17 in
  match Overlap.array1 ba1 ba2 with
  | Some (len, x, _y) ->
    let ba3 = Array1.sub ba x len in
    ...
  | None -> ...
```

`overlap` returns the length of the shared area, the `x` point from `ba1` and
the `y` point from `ba2`.

## A `js_of_ocaml` support

`overlap` is compatible with `js_of_ocaml` but it lies about expected results.
Indeed, we are not able to know the position in memory of given `bigarray` in
the JS world. By this fact, `Overlap` with `js_of_ocaml` will always returns
`None`.
