# pqtree.js
An [emscripten](https://emscripten.org/)/WebAssembly version of [Gregable's implementation](https://github.com/Gregable/pq-trees) of the Booth-Lueker algorithm for producing [PQ-trees](https://en.wikipedia.org/wiki/PQ_tree) for the consecutive ones problem. The paper "[Experimental Comparison of PC-Trees and PQ-Trees](https://arxiv.org/abs/2106.14805)" by Fink et al. performed an extensive computational study and found that implementation to be correct.

## Graphical online version available at https://pref.tools/pqtree

[<img src="https://user-images.githubusercontent.com/3543224/146847918-2d2130ee-c6c6-4097-b381-a52d96a3d9e7.png" width="600">](https://pref.tools/pqtree)

The sources for the online version are in the main directory of the repository: pqtree.html/.js/.wasm.

## Build from source

To build the emscripten version, assuming emscripten is installed so `emcc` is callable:

```shell
cd src
./build.sh
```

Use it as follows in HTML:

```HTML
<script src="pqtree.js"></script>
<script>
  var process;
  PQTreeModule().then(function (Module) {
    process = Module.cwrap('Process', 'string', ['string']);
  }
  
  message = "6;001010;011010;011101";
  // 6 columns, semicolon, then a semicolon-separated list of rows of the 0/1 matrix
  output = process(message);
  // output == "(0 [(3 5) 1 2 4])"
  
  message = "5;11000;00110;11110;10101";
  output = process(message);
  // output == "Impossible"
</script>
```

The `output` is either the string `Impossible` if the input matrix does not have the consecutive ones property,
or otherwise the induced PQ-tree, with P-nodes specified in parentheses `( )` and Q-nodes in brackets `[ ]`.
See `pqtree.html` for a utility methods to parse these strings into JSON, and a method to extract one or all orderings induced by the PQ-tree.
