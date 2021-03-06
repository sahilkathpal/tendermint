# Generating test data

The easiest way to generate this data is to copy `~/.tendermint_test/somedir/*` to `~/.tendermint`
and to run a local node.
Be sure to set the db to "leveldb" to create a cswal file in `~/.tendermint/data/cswal`.

If you need to change the signatures, you can use a script as follows:
The privBytes comes from `config/tendermint_test/...`:

```
package main

import (
	"encoding/hex"
	"fmt"

	"github.com/tendermint/go-crypto"
)

func main() {
	signBytes, err := hex.DecodeString("7B22636861696E5F6964223A2274656E6465726D696E745F74657374222C22766F7465223A7B22626C6F636B5F68617368223A2242453544373939433846353044354645383533364334333932464443384537423342313830373638222C22626C6F636B5F70617274735F686561646572223A506172745365747B543A31204236323237323535464632307D2C22686569676874223A312C22726F756E64223A302C2274797065223A327D7D")
	if err != nil {
		panic(err)
	}
	privBytes, err := hex.DecodeString("27F82582AEFAE7AB151CFB01C48BB6C1A0DA78F9BDDA979A9F70A84D074EB07D3B3069C422E19688B45CBFAE7BB009FC0FA1B1EA86593519318B7214853803C8")
	if err != nil {
		panic(err)
	}
	privKey := crypto.PrivKeyEd25519{}
	copy(privKey[:], privBytes)
	signature := privKey.Sign(signBytes)
	signatureEd25519 := signature.(crypto.SignatureEd25519)
	fmt.Printf("Signature Bytes: %X\n", signatureEd25519[:])
}
```

