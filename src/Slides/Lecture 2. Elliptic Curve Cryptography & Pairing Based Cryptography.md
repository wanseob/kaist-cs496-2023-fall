---
defaultTemplate:
  - "[[cs496-2023-fall]]"
---
### KAIST CS496(2023 fall)<br> ZKP - Theory and Applications
Wanseob Lim<br>
PSE, EF

---

#### Lecture 2
#### Elliptic Curve Cryptography<br>& Pairing Based Cryptography

---

::: title
Recall
:::

- What is $\mathbb{G}$
- What is $\mathbb{F}_p$
- What is the scalar multiplication of ECC?
- What is the discrete logarithm assumption in ECC?
- How do you create a shared key using ECDH?

---

### Elliptic Curve Cryptography

---

::: title
Elliptic Curve(Weierstrass Form)
:::


::: left
$y^{2} = x^{3} + ax + b \, (\text{mod} \, p)$

- General Representation
- Complex Group Law
- Widely Used
:::

note:
- standard elliptic curve form
- point addition & doubling operations involve multiple edge cases: which can be computationally expensive
- most widely used
- Has a point at infinity which is written in $O$

--

::: title
Elliptic Curve(Weierstrass Form)
:::


::: left
$y^{2} = x^{3} + ax + b \, (\text{mod} \, p)$

- Addition Rule: $P + Q + R = 0$
- 
- Complex Group Law
- Widely Used
:::

![](https://docs.google.com/drawings/d/e/2PACX-1vRd1OwnVIt205GDrw8GRERl4SArV5oYTvVYF4MGZzx3EkSRcEWd29a4VtBM76ewE-0fjpDOXHCoi2TV/pub?w=377&h=492)
<!-- element style="width: 30%; position: absolute; right: 50px;" -->


--
::: title
Elliptic Curve(Weierstrass Form)
:::


::: left
`secp256k1` (used in Bitcoin & Ethereum)

$y^{2} = x^{3} + ax + b \, (\text{mod} \, p)$

- $p = 2^{256} - 2^{32} - 2^{9} - 2^{8} - 2^{7} - 2^{6} - 2^{4} - 1$
- $a = -0, b =7$
- $G =$ `0x0279...1798"
 - Order $n =$ $11579\dots3951 \simeq 2^{256}$
 - Cofactor: $h = 1$
:::

note:
- Prime number is chosen by some [[primality tests]]. And use $2^n$ as much as possible to have nice bitwise properties that allow for optimized arithmetic operation in the finite prime field

--
::: title
Elliptic Curve([[Weierstrass Form]])
:::


::: left
`secp256r1` (used in TEE/Secure Enclave)

$y^{2} = x^{3} + ax + b \, (\text{mod} \, p)$

- $p = 2^{224}(2^{32} - 1) + 2^{192} + 2^{96} - 1$
- $a = 11579\dots53948$, $b =$ $41058\dots1291$
- $G =$ `0x036B1...C296`
 - Order $n =$ $11579\dots44369 \simeq 2^{256}$
 - Cofactor: $h = 1$
:::

--
::: title
Elliptic Curve(Montgomery Curves)
:::


$By^{2} = x^{3} + Ax^{2} + x$

![](data:image/gif;base64,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)

note:
- All Montgomery can be converted into Weierstrass form
- But not all Weierstrass can
- We work projectively. using $Z$

--

::: title
Elliptic Curve(Montgomery Curves)
:::
$x_R = \frac{(x_Q - x_P)^2}{(y_Q - y_P)^2} - A - x_P - x_Q$

$y_R = \frac{(x_Q - x_P)(x_P + x_Q + A) - 2 B (y_Q - y_P)}{(y_Q - y_P)} - B y_P$


--

::: title
Elliptic Curve(Montgomery Curves)
:::

::: left
$By^{2} = x^{3} + Ax^{2} + x$
- Montgomery Ladder + Projection $Z$
- Complete addition laws (no exceptions),
- Birational equivalence to Twisted Edwards Curve
- No Point at Infinity
- Can calculate only using $X$ coordinate
:::

note:
- **Efficient Scalar Multiplication**: The Montgomery ladder technique allows for efficient computation of point scalar multiplication, which is the central operation in many cryptographic protocols.
- **No Point at Infinity**: Does not involve the point at infinity in the curve equation, which can simplify computations.
- **Limited Operation**: It only supports a differential addition and doubling operation, meaning that it is well-suited for certain computations (like point scalar multiplication) but not for others.
- Can be converted into Weierstrass Form for some special cases

--

::: title
Elliptic Curve(Montgomery Curves)
:::

::: left
`Curve25519`

$By^{2} = x^{3} + Ax^{2} + x$
- $p = 2^{255} - 19$
- $A = 486662$
- $B = 1$
- $G = (9, y)$
- $Order = 8 \cdot{2^{252} + 277423\dots48493}$
- $Cofactor: 8$
:::

note:
- private keys: $n \in 2^{254} + 8 \cdot \{0, 1, 2, \dots, 2^{251} - 1\}$
- Reason: Not every 3232 byte is a good key. For security reasons, the last 33 bits should be 00 (this prevents subgroup attacks). To prevent side-channel attacks using other scalar multiplication than Montgomery ladder, the first bit should be 11.

--


::: title
Elliptic Curve(Edward Curves)
:::


::: left
**[[Edwards Curve]]**<br> $x^{2} + y^{2} = 1 + dx^{2}y^{2}$<br>

**[[Twisted Edwards Curve]]** <br> $ax^{2} + y^{2} = 1 + dx^{2}y^{2}$


![](https://docs.google.com/drawings/d/e/2PACX-1vQILUglrs40LDGcNTHBB-ZmzFcUUmS-UjTR7e2U1WdQGxhIY-NsJAiilJQwsfdL6wAJtx1a_5OoCOIh/pub?w=489&h=492)
<!-- element style= "width: 35%; position: absolute; top: 135px; right: 0" -->
:::

note:
- **Complete Group Law**: The group law on Edwards curves is complete, meaning that it can be computed with a single formula that works for all points, avoiding the special cases that come up with Weierstrass curves.
- **Fast Arithmetic**: Edwards curves allow for fast addition and doubling operations, which can speed up cryptographic protocols.
- **Secure**: They offer strong security properties, including resistance to certain types of side-channel attacks.

--
::: title
Edward Curve
:::

![](https://docs.google.com/drawings/d/e/2PACX-1vQILUglrs40LDGcNTHBB-ZmzFcUUmS-UjTR7e2U1WdQGxhIY-NsJAiilJQwsfdL6wAJtx1a_5OoCOIh/pub?w=489&h=492)
<!-- element style= "width: 35%; position: absolute; top: 135px; right: 0" -->

::: left
$x_R = \frac{x_{P}y_{Q} + x_{Q}y_{P} }{1 + d x_{P}x_{Q}y_{P}y_{Q}}$

$y_R = \frac{y_{P}y_{Q} - x_{Q}x_{P} }{1 - d x_{P}x_{Q}y_{P}y_{Q}}$
:::

note:
- Found by Euler in 1791


--

::: title
Elliptic Curve(Twisted Edwards Curves)
:::

::: left
$x_R = \frac{x_Py_Q + y_Px_Q}{1 + dx_Px_Qy_Py_Q}$

$y_R = \frac{y_Py_Q - ax_Px_Q}{1 - dx_Px_Qy_Py_Q}$
:::

![](data:image/gif;base64,R0lGODlh6QHvAfcAAEBAQD9OZD1UdkNDQ0FBQUdHR0tLS0pKSk1NTU9PT1JSUlhYWFZWVllZWVtbW1xcXF5eXk1cc2FhYWJiYmhoaGtra3R0dHV1dXp6enx8fH19fYCAgFpuil1yknZ/i3uGl3WBlF6BtV+BtV+CtmCCtmGDtmKEt2SFuGWGuGaHuGaHuWeIuWiJummJumqKu2uLu2yMu22MvG2NvG6OvW+OvXCPvXGQvnOSv3KRvnWTwHeUwHeJpHiVwXmWwXqXwnuYwnyYw3yZw36axH+bxICcxYODg4SEhIaGhoiIiImJiYyMjI2NjZCQkJGRkZOTk5aWlpiYmJmZmZubm52dnaKioqSkpKampqWlpaenp6qqqq2trbCwsLGxsbOzs7W1tba2tre3t7y8vMDAwL+/v76+vra5voGcxYKdxoOexoSfxoWgx4ahyIiiyIqjyYukyoulyo2my46ny4+ozI2mypGpzJKqzZSrzpasz5Wszpatz5euz5mv0Jqw0Zuw0Zyx0Z2y0p6z0qC0056z06C106K21KO31aS41aW41qa51qe61qe616i716m82Ku92Ky+2a6/2q/A2q/B2rDB27HC27PE3LLD3LTF3bbG3bjH3rnI37rJ37vK4LzL4L7Bxb3M4cDO4sHBwcjIyMXFxcrKyszMzM7OztLS0tbW1tXV1dra2tzc3ODg4N/f397e3sLP48PQ48TR5MXS5MbT5cfU5cbS5cnV5svW58zX58rV583Y6MrU487Z6dDa6dLb6tPc69Td69Xe7Nbf7Nff7Nfg7djh7dri7tzj793k797l8N/m8ODn8eLi4uTk5Obm5ujo6Onp6evr6+zs7O/v7+3t7eHo8ePp8uTq8+br8+fs9Ojt9Ojt9enu9erv9evw9u3x9+7y9+/y9/Dz+PLy8vT09Pj4+Pb29vT19vH0+fL1+fT2+vT3+vX3+vb4+/f5/Pn6/Pn7/Pv7+/z8/Pn5+fr7/fv8/fz9/v3+/v7+/v39/f7//////wAAAAAAAAAAAAAAAAAAACwAAAAA6QHvAQAI/wD1CRxIsKDBgwgTKlzIsKHDhxAjSpxIsaLFixgzatzIsaPHjyBDihxJsqTJkyhTqlzJsqXLlzBjypxJs6bNmzhz6tzJs6fPn0CDCh1KtKjRo0iTKl3KtKnTp1CjSp1KdaqoJAzuCcwQparXr2DDulxmBYA0ffgIdBHLtq3btxaXAWCl71mCcnDz6t2b1xyAUvqorOVLuLDhqAXCyMOA77Djx5CFNshiBVXky5gzz6yQpInmz6BDk9RgIJro06hTU8SARbXr17AJ3rsQL7bt25+NhNqSCrfv34fjVVAABrjx48iTK1/OvLnz59CjS59Ovbr169iza9/Ovbv37+DDi/8fT748ZCvm06dHr769ePbu43eHL78+9iKj7Ou3Tn+//+f9/SegcgEOaCBwBR6ooG0JLuigag0+KGFoEU5oIWYVXqihYxlu6CFfHX4o4lshjmhiWCWeqCJV+K3oIokvxshWijLWeBSNNuYoFI469tgTjz4GiROQQhY5E5FGJmnRM09YoIBpBnUBwJQADCBOQUgqqaVE4kABADQHXfHEKWSqYlCWW6bpkBdfHhQFKQqhqeacCbEJpkFLLBMnnXx2ZOdBRWRhxAQU5Idln4hm9KdBU/SGzxEDrHJoopRStGhCck1REH6jGFrppwxdihA8ABwxKaioLiSqVuJQoZU+0QD/UMWpqdZqEBcAODPQEQ7AYwoAXwhEBQJnESSnrUU6M4UEAGBAhUBJSACPPmFUsEQTGjBzJrLcZnRst7V+Cy6q4o77abnmUopuuoiuyy6f7r47Z7zyptlivfgORG++Se7Lb5H+/htkwAL3SHDBOR6McI0KLxxjww67CHHEKk5MsYkWXyxixhp7yHHHGt4LcrgjI/txyRKejLKDKq+sYMsuGwhzzALOTLN/Nt+sX84618dzz/H9DHR7Qg+dnshGp1l00uQtzfR7T6vpdNTfTU31fFdraXXW2m3NNXZef82f2ACTLWTYZkuHdtrQIc22jGu/3Vzcci9Hd93J3Y33cXrv/42g3w8D/h+TTkJpUBYZILHBM9sK7l+XbRqkBQJXQvEAObQ6bp+oAy2ghEDNADBG5prLx7k+qwDAxUAHIEF66e6dXgoAoAzUwAUEuePBObBvHjlBoAAQykASUDBQN0EEAEIZnvaunuy02467PsC4EIIAIZgAi/Ox/z5Q6qsLZMAR9zwyQggvfJBDCCH08Q736a2qzz0LLAG66JiwL0Q3VqwDB/uCgJ95cKUrgfBqWpMbhz6k4ABzzIB970PPPYSQvXAIUDzKYpazoCUtgWRBA0pYHCxCQIJuCIQ97nhBCBxxwf/k4wchuIO+BvKIELSgHS3czwhDkIwZCgQdJggBJ/9yaJ92qJAOxiLIH0JQA3sQUT6aYJ83kjiQarDvFU90zz1uEIJAZC4PIbDBPLKoHlywrxqZ20YJQkAJMqYnDiG4QeMI8j8d5MON5EHHGmsxx4Eog309xKN4MhGCFLjDIG7LRw+6KMjwvDAEfjhIgSxhwzE20jvIYJ8xJHmQcKxRFpf0ziBCQIM79pEgbQjBGkLZHR2E4BEISZAsQnACS7ISO9TQZCwRgg4RhCAWt8yOIML4qlMShA0hUEMwr+OOFYRAEwlpEC/Yt41lVocWISgBOqKZEHvQ4JXWpI4cQuCGPSXkECHgQTilM48ThGB73EzIMai5TuhMcwTbjCdC7gH/g2fW8zl3CEEaFlKhPoRACP9sTjvcOUSFuK0gv2DfNRK6nE/Qkh0EXcg9cBCCRVBUOWkIwR4YkiFJhCAG9fjocbzBvl6QlCHbYB8wVGqcGrogpRllCBFCUAea/sYeNgiBIRrSIU5kTx0+xY0v2IcNojakHSgIwSaSehs9hGAIDgnRHkJwBqrGph3sm6pTGxLREDTVq65xBfvy+dKG2KOfLESralKJh4eUyBHElCtqwuHLXNj1IdfQpV5FQ4kQrIAef32ID0Q62NDYg6OEgEiKCHmCdDT2M7uQKEQemhB2RBUTl9XM/9oQERoRIgQ5MGVoIcPSEIBSshH5YwiCsdrI/zQiBC5ALGwjMgSe1vYx9YhBCBohERyNsAQW/K1hbhECEZiwtBKZRwvYqFzDwPENE+EROlNbXb6EgwQhsEV2J5LLEBSju3thBPp0C92JUDAO6M3LOpwZ1+JSRK0imGh83VJDFbC1vRKpBxfzsN+2qIO+FSHSK5qr3wKDJRKF/C+AA7w+PTgYLPOwXiEswlmHeKK537iwVwpbgikm2CLzUGEkRTwVdDgzshbJUv5GgEYWR0UR2ZOwfS1Cj6Ai0cZPOYc723gRNNWCfdQAslNG2QKMFhkj93AlaZW8lGuskchPxkgs2PcLKivlDXFkb4wzco/e8gCnXi7KUkNwi4EgTv9xjCuIlKhkpdeR93yeSHNR6MFFNZhycpW7XEHERKZTmMnOEwFECFDAOz0LpYYkSLJAPHe/0RHkTebMyDmmK0NHA6Uaa0zE91THOtcRJE+ZzohFQ8ALT/vEHju9wSEFMrvaCeR2mxIUoZp3Qo7kYw1MdLKrdZK/EMx0IMEbnkCKV5BGoQVSkqLiRrSxxgAOOyfYcOcfClJr6WEKAJoiCKd4bZHChqDL17ZJPXprA2ELBHwDGV9CSGUqaW+kHmYIwQzWke6a3NbYBqGf/fQROkuzylUCidWs7L2RbIfADqrt90t6wT5FICSBC3QA5g74q2AFhliIvsiq8yzxl3RjukL/EHNBPhjCOEdrWtW6VraMqZF81CGbmyw5S97R2xY89yPxasf6buBunZskH1YVgS9EQi9qBFENKjf6SGpIXaaPZNV3KKbURbJqP0Qc6CSBBPtEvXWR8OJ8c0BzSPyVj9P6s+wfCQb7foBDkgTMHnB8J9w70gt32iC5djeJO3obApLvHSO5CGIOfh54k7DjDOzLxOEvcos19gDwJenwR9qRyhCAdvISyccm2NcDHY+kYfPovCKcCHqH1CMQ7POB6U+vktSzbw5Fb/1B0oFMntY9JROrBzpD0IOz6v4g2KgB+yDx9ZNkzBVrXIFfjy8bTLizBFhsCceI0c8QHCLqkw+H/xvYVwPauuRj3ghpCILQYND/Qgbs40PuVXKyepivkMCcPDfAaNjXwqRlw/BAIdAGkrZ18+AIQXRQ2XAkM4EOYNZcf2BZRucLrhQCMuAKrCcTM5MPvLBIJwULzedo+eALaMA+JcAIv8eANVEPmJACcycLapdm95ALFMQ+ZlCANfEz3ZB3cfQJ4Cdi9AALwsU+bPALIaiBOpEMdeBLhYQJKXhh60AJAhgCczAMPKF5MnENBsU+LeAIjbZf+UAMfXA+JFQHyOATVqMNf+BO2RMINVZd30AJXMQ+KpAIjMcTXnMOjzBdcxcJ7TdY6OAJasA++rMJ87cTaNMOmsBRhAgEl/9whz71DZqgflxYCG+4I0RxD8WACN9EiD8ACdagUvQwDI8QBIRYSHvQCzGIiUaRD8UACCxwij0wCdRwhIJ0D9bACXKgAqfYAn+QC++TFH0jEPTwC4oQVIT4AnWACciwikRUD8iACWvAh4ToA4jwC85oFMNIEPlwDIYwheyDAmgwCcNgS/AzD8RQCW0QVacYA3XwCZjXFNtoEPlwDZ+wB4xIiCYgBIQQC9xgi2zjDbWgCG9wA0xIiDiAB5yQDQCpFPOoEN4gC3rAA6fIPivQBoUAC9eQgVxTD9vAC5hACG0QixUZAjrwB7JgYl/xkA2BDrfACGngTBVpAj1QB5BwC9b/8IMrgw7E8AmKIAc1sEYleQJmUAibEAzxCBYsCRH3UA2fgAgxWZIhMAIxMAeFwAnAAA4NiS/0cA27EJJ1EAQqJJUmwANxcAickAzZKBZLSRH3oA27cAl24ANsWJIrwANyAAiUYAvUMGvj4g7Z0AuewAh9AAdmAANkKJU1wAaAYAm7wA0cSRhY6Bb34A29kAmDUJCJWZItIARvoAeIoAm2UAzY8IRFUg/ngA3FwAu0cAmMYAdr4AM0UJdSSUs+8AaE8AiwgAym+RhtCRLzUA2zMAmFEAdCwIu1SYgn0ANqUAeIQAmwsAvFYA3hoJMKYg/ooJq8EAuZEAmFkAdvYAYV/5icFUkCN4AGe4AIk+AJwNANWkchqZEP4UAMrkAJiMAHa6ADtEme7JMDQ9AGdpCbmqAJuVAM13AOa4kd9JCaxtALtTALs4AJj0AI4EkEPEADLXCQ/KmcNOADbLAHj6AJsdALx7ANCYoavwkT89ANxSALmxAJfuAGQFAD7Lihp7gCMcADQtAGeBAIjXAJm/AKssALBhoO5vgZ9gAO1lAMvZALtjALsuAJl+AIhPAHfaAHcvAGZ8ADMuACJ0ADGmqjyvkCOXAGccAHikAJniALvVAN4HCkBPIc9BAO1vALs+AJlLAIfkAHaXADI+CCYlqSMHACLwAD/tkGecAIikoIjf9gCZ5AC7OAC7nQC8VQDd2ADuvQDvQQmTiRDp6wCZqACZDACI/ACa7gCpMACHXABoiKB3/QBjoQAydAAi+AAjgglIFKnkFUAi6QA835B4xACZrwCbUADNTgDX7ZNd0xp9UwDLbwCZ5QCY0gCKsKBDlgA16aq7lqAmS6Bn1wB3rwBmsgB3bQB3/gB39ACIiACI4QCZbwCZAIEtagrWKaAi0AAzRwA0LABnCQB3vwB4GgCJXgCbPgCpwAC79QDd8QjPKRomExp9cQDC4KC1FKCYyQB2ywo2rQBkOQAzUQAy2AnPTKn7hgEtyQAzzgA0JgBkKQBjrQAjJgBnYgCIxQpXL/UAeKcAlDuguzMKxtGg7vsJUscyILig3VoAzHUAzA0As92wh5AAd9sAiFcAh+sAd5cAd2YAdtsAZpgAY9YAPnFTMOS30YMbZkO2Znexhmm7bjxbaFMZluiyJxSxhrO7dZZbd7Ubd4m1N7CyN967d/2xZ6G7g0JzbkxhOHi7hCUQRCkbg74bg5AblKIbk3QbmVu7iNOxSWWxOb2xFjUAQVoA/ioAEN8J76YLmUm7oNoboMwboL4boKAbsJIbsIQbsHQbmM27q6+7qru7u8+7vAO7u9G7zCS7y167uxi7wMsQy4Mg1JAAqWVhCoq7y3O7zJS70GMb3GW73bm73Y673d/yu9DJG74TsQtgu+11u+AqG96du+x/u9BMG+xau+pxsR5TAAFtAKWGIF/Nu//vu/ABzAAjzABFzABnzACJzACrzADNzADvzAVgABTDDBEFzBFnzBGJzBGrzBF/wQFUC+5jK44oEPEzAB7yLC4YEFWUAA0nAPBQQuKNwdq3AFWrAF5FAAX7AFdDEuMcwdoIAATKAVT8AAg8HDhCu4RzwjaUM4T3IQczYldfYSTGw4K5c4izMTb3bFckYlVXIlKogTWRxnBPHEXQwTU4wQYSwTZxwlXBzFRgE5dzJoY1ImMgHHBwFo+mA5mAMTeKzHclxoh/bFNtHHgkYQhEbHMWHHkv9DOXlcyDChyH+MyEhxOvqAaTZByZRGcKITE5lccAVhyTchNJ28yZcGJ5fsPZ3zOZocvTBByaC8FJSMaqccx6MWPvrQOjABbwKBy6emJ0OCE7p8y6Y2ELJcE6cTzLwcE7Hsy0xByYEyKIUyE9Bja/qAay/Rbbc2PQPxzLuWgziBzdWszQLBzdEsE9PsbeaMyuOsa+U8yeqsD872KJGSzrQsEMlGPMbzEve8bPk8EPEMbTQBtyuxz/rAbATxz/OszOpM0Aat0PUsEAgdbToRBU5Q0RbtBNoyEJRMEJlSEhR90RWd0QJxztlsEh8N0toCztZ8EB0tyDSh0uJcEC3dyur/DNPS/M4cDW48IQpi0NM+LQbFMtI4PRD0VhI8/dM9HdT6cMykJj71RhJHjdRnEczyNiqlQhM/Q9VPbRBF7dAGodU3/dAF0dVHIT+t8ioKR8+yMT/1U2kwIXBuPT+ii3CwIitYjRNwvcoCcXBobddePRBakdeerNaAPdd9vXBHQUC70isdJywgFxOKbUC9og8Yx0B7/BKVrXH6wHHA4thK/X85kdkbx9id/XGf3RKRvdmTLdozkdqc7XHDctpBkUHN8iz68HLUYi3YItIuQdsbdNsdpA8sp8UxMdwu10Ext9venBPGzUEwp9szBxO+bdu4LdwgRNwvMd3Ondsyx9sx/5EPpjsvW/IO4bCK9xDeH4ENzWUCKNACMZADPRAEQ3AGGysHa4AGOgAEcfAHkIAJlDChiaAJtTAMyqAN3hAO6dAOJyozG1IP7aAO5/AN20ANxGALgxkIjjAJmiAJg1AHHWsGccAGbqADYxkCLECoKGACfWUS8zqyuXoCXWoDKfACPwAHelAIj5AIh9AIm1ALv5AM3IAOlyq03NHDSrEOQd4N1BAMtsAJj1AIjOAIhrAHciAEs1kDL0ADNeriNtpmJdEOw7C0POsKnhCqllAJj8AIg0AIkDAJllAIcqAGQMCcZtAGPACoXF6bMsCcctAHgYAIkbAJs/Dj3cAOnEodRv+eE/fADt5ADcBQC54wCYlQCHpQB2rwAzWwmXl+iieQA2xwBrHpA2kQB4FACZNAnIGwCJQgC8WADMJQC64wC7fQC78wDLO3E/WQDt2QDdZADcgQDLsQC5vwCa5AC5NwCHwQ52cgBGtwAxnK5S/AA2ggB3xgCI9wCT6uDN3QDkQOG4muEvnQDo3O5KKKCHuqBj0whC6+AjaABkKABmhg44UACa4AC66gCa+QC8CADNRgDdgQ5NaZGfegDtugDNegDL0gC5oACZROB21wBjrgAmG6oSVAA0NgBxiOCa/QC9cAp67x7R/hkb/AnRN6Bz0AA7gaqCxwA0SwBnRwB7mJCbD/wAvIgA3KkA3osODZcQ/pkA1hDumRgOxxoAPOPvFSKQIvMARzwAeJsAm90A2HnhkgXxH58A3C8AmM4AdxcAYyAF42ugI3cANzcOOPIOi/AAzXkPMjMvDbcAy54AqPoKpq4APgSJY4wAZ58Ai0kAy9qbaZEZy4MAl70AY5sJ8lGQPMSQd+0AiewAvGgA1qPyf20A3CUJ+J0Adq4KfJWQNr8AeUwAvaoPNiIdBhcQ/d8AuYMAhtYJCbfwZ6cAjr+Quh/y5d+ZWMcAdoQI0VWQI6sAeVYAv/qBdTrw/hEJeAQJe1qQJDgLOXwJd9jzDoUAyuwAiwmvKEqAJtcAivoA3d/y6PbvENtcAIcFDipzgCN6AGiOAJwaCVRuORuSAJeSAEmm5YbvAIwPD8TzG49FAMlCAH1lOSAOGjjqNa1+jpQ5hQ4UKGDR0+hBhR4kSKFS1exJhR40aOHRnaq/YJURoWIUyaLBGkUC50Hl2+fGgF5kya7X49EnLipMkTQwhxQnaQ5lCiRY0eRZq06D1rn/wAEbEzRI9AtL4pxbpQZlau6GwdGkJCqg49m5LV45pW7Vq2bd0iVLeLEJoSUtNoCvd26Fa9MNe9elP35Aghfmrl7ZtY8WLGjRG+GzbJzU4Raixdc5yRb+aK3jYF3qmmUS92nE2fRp3aZb1ee1bsDIIJsf/qhptpK1TnSk1UnnVqubsdXPjw4PR48UlxUkSbWkKF26at7lUcsSfrzGpHXPt27o7zEQPU4mQLQ9aeC+dGSCfPO7zQdocfXz5berYmn3wDLB9t6KaR2RnBOuzmI7BAA5ECx5IaTgJilntS66+xfHpB46QaMCntQA035LAje25J46QeZHmPsyJGMe2eXMw46YdYSuwwRhlnhOiYNk66gZN3OIuwr3pi6eGkIHrZj0Yjj0SSGjt4myGXzHp8q5ggTXqDGCSvxJLGa/w4SY5uGoOSLXT+4M0OZbJEM00Ot7kxBBMYWWexMNOa5xLxQgACGTX35JPAfGCRwSQZhlFszqz/ktHBpBUugbFPRx8drp1ITBIhEnv6MjSpey4RrJCWIAU11OC2EcIkNLbRK9OjwmlThzNFhTXW0+YhhKdP3lK1qF5cMOmP7GQFNljGeqHBpEaKXCvXoT4JkIVahIU2Wr3SUcOkO3ZMlq17GDHJB2ykBTfctOjhMoQgvMlWLXbeMKmNOMWFN16j8sEkqhi+TUtZl86ZkpBG5QU44I56MQkGzLjStyN+TbpVYIcf3giZkmCohqsTswKHhxBKcBJijz+mKBnxWEgmq4Q16gYHN3cBuWWXHaLmhRBUqFipkzEKxwY3e3m5Z5/1uSZAGWY76maL2ik1BEJ/ZrrlZFAIQYhf/4tOqp44Qhih46a39riXutpwriijJ8pnD5M84Tptj2ExKRCkxpaoEZMcUbvuh+UOwRWqjfolqjyQtTtweO+5+oRXHcoiAyQ2eIahLgCAHIABxNHKqHBiCGGIfwXnPNp1bgjhhncZ0gIByqF4gJyFrnjiFNdVYQhuh+hhkQVtOsddXGrEwgPwhBZQAqFmABhjoShIgUh2hvIpl5fcnwfXE5NkYWgVALhI6AAkFlpimeSJwsUkSaAnH9p8rm6BaH1KAQCUhBq4YKEisjBiAgpQrLymBc/wvXz/Qz2HzN4AOFAAIBQJkQAFFjKFVOgDH0cYwCrkNwoKwuQPITgBN/63Qf9RRcEJIIhABDzgBCcwY33te1/8HLIMAEwhfzDhhUkmwUEaQkoUYhDDBzjQgTKIQRr6sB72EGKAIzwEHgAookKUh5BwyAwNm6thFNEEjtfoISH3WMAShEc8hDxIHFR4kD6iAYAqvNAl5fqSFNXIp0+Y5BcJKd049CEFB6juCA6AhykA8AWEUAEBP1QiTKwRlU2s0ZBqugcRplKiLGhACYxDSBIkAA99hKECS2iCBkxoxo7IIQQ5gOIhRSkjZUQFbS5R3i9MYotRtvJKfAjBC0bHEdnRI1Fp6J8rdbmhcOgEEah0CSZMQo1dFlNGj8Cg+jTjEXoUiw/GhCaH2iGzQ3j/BG6b2Fgao7nNAlECa9rUyNjawas/cNOcBGrHDEJQh46MjVsnAOc55bkdV5jEPBsxGjuSs4h59pM79gDdHjhyMY148wTq8GdCiaOJjaErnBt5R6AKoVCKBieiIRgEPjciTBNcpaIfTQ1DS+BQjJzMHZibKEhVapp5FCujy8wIRz26Upo2xhIhQMHULJKwfAS0pj9lDDt0oreSZmQYJqkZUJWqlzyE4AwwvUgforZUqr4lGEgt6kWEGoJTVtWraclHDkJAiKxahKEoyNBX1YqVm54grRRR1j3EWs611jUp6TjbRZR11RAczq5/JUpTiaDXi+ghc4BFLFGKYRJjWISg/xMJR4CImljKuiQfQAhBHnZqEUiEwAXAqWxoOyK9E+g0IqrqaQhSKlrWZoQdJggBLCqiqmPYs7W3xUgdQvCG2VakEFPBbXArYguTkPS0FJmHTighXOZGhB7KhStFbmEScDTXug4ZRAh0kMvYUeRqbbhueBdS2xDoSSKGOkddqCde9ubDByHow0QM5c0VYIu94hVmCuwbE4nkQ2N0va940SGWZx03ItQwyTECfF92yeG8EklE6Li74OCGbwTV/R5E6MGrGVJYvBsOwXIh8tiGWNi4HrauIMxl4Ifodg0oZu9RQ3BiTi6EHirIG4zFWw/xcCLDDgGGSZSpY+E2FQ0/bv/IBYdAZPHmIgQioHFCejQP8VyCyeGtBwxCQDeH9OgVISDBkK+MW0WEQAZhC6RDzhCCOIw5vNwwSTC67JBumIRlbr5uqV7a3YbcdAWhxDNrFxGCGYSxxvrIhyLZGWjrYsMkvqhNQxarNEZft1qLPrSKYzDhSov2yyVA6AQXco8FQaLT1m0H1BqWZoXIGF+nZq5hbeA76LBoybBu7qSLwclwmMTKuGZuPkqFB04i8wX7BTZuY4FVKSsEHXNLdnPpIVb+NTshUmWBmKMtWl+40dr6aAdsx7fttT7jCRZQQDQQpzjGVcsN375pCd5Kbq+KAwoAgEZDSne6B+iC2VtpR6D/AEFvu3oB3w0B3hbJoLEQ5EUm7VjQCWZK8K8aPN8LCWL2kCALk8zAGFbYxntD8AiK19XiDGGf+xACP33MAscm4EBJSHAJTpe8pidfSAEPiJAEIuQaiRJALJdmc5B6kIRHL6FCcK6QlKcwIeyIQwB20An8Ef2jN8Rh1n2o9INj/HoJIaJC7vEBZFu9qkvXx4OwqEV9DK94rDb7V7kAAGck5I6UjOMc63jouP/UGVOQAAAwQIVIThIhjXxk4/je99sukfFKdfzjfxp5ydOU8pVX6eUx/1HNb56infd8QkEf+n6SmPSUHf3pz5l61XOT9a2P5uthb0zZz36Xtbe9K3Gf//tR7p73h/T979cYfOFLkfjFr+Hxkc9B5S//f6Z3PlCbH33yTZ/6z7P+9XGXfe1zjvvdD9z3wV838Y8/beU3/9bQn36mrZ/9PnP/+18Wf/m3DPr1nyf98e8x/e//Yf33P4EBwAAEmAEkwHgxwAMUlwRUQHBhwAaMlgeEQGGRwAkElgq0wFjBwAwUlQ3kQFC5vw8UJQ8UQUchwRLkkxNEQTVRwRVEkxZ0QSyBwRgcDnNDN3VznMiRHMqBOxpMG3vrutVpndfhMx+sG7RLiONBMiPkGiREiO5ZQiZsGifUh/mpn/spQincGipkIAeCIAnqQS0MGKNDuqRLCCpUCBZyIbSFOJEKEsOAwTqt27ozDEKHOKIk+rY3/Bm08yIwQogxKqMw1EOXmbu6Q4i70yM+0gc/AqQ8HMSW+bvAG7zCoyRLwiRNysJHLJ8Z1MQC4cROnI9PBMX4EMVR7I5SNMXtQMVUJI5VZMXzeEXcCcFY5BpXpEXVsMVbRI1c1EXT4MVefBJgtJtfFEbGIMZiLBRkPD9lrEVmVD9nbJpjhMa2kMZpXItZtEaHqcZsRBhunD9vBEeiCAgAOw==)
<!-- element style= "width: 35%; position: absolute; top: 155px; right: 100px" -->

--

::: title
Elliptic Curve(Twisted Edwards Curves)
:::

::: left
`Ed25519`

$-x^{2} + y^{2} = 1 - \frac{121665}{121666} x^{2}y^{2}$

- `Curve25519`'s twisted edwards curve version
- EdDSA has some security advantages
:::


note:
When to use Edwards: EdDSA
When to use Montgomery: Key Exchange / Scalar Multiplication
Why it is called 'Twisted?': Almost same structure so gives similar properties

---

ECC Applications

--
::: title
ECDH<br>(Elliptic Curve DH)
:::

- $A = aG, B = bG$ where $A,B$ are the public keys and $a,b$ are the private keys
- Shared key $k = abG$

--

::: title
### ELGAMAL on ECDH
:::

<span style="font-size:0.7em">

- Encode message m into $M$
- Alice & Bob's key pairs: $A = aG$ and $B = bG$
- Alice chooses a random integer $k \in [p-1]$
- Compute the ciphertext pairs $(C_1, C_2)$ as follows
    - $C_1 = k*G$
    - $C_2 = M + k*B$
- Bob now decrypts the message $(c_1, c_2)$ as follows:
    - $M' = C_2 - b * C_1$

</span>

--

::: title
### Schnorr Signature
:::

::: left 
Given parameters:
- private key: $k$
- public key: $P$
- challenge: $e$

***

What if the signature is $s = ek$?<br>
It's very easy to retrieve $k$ from $k = s/e$

***

Instead
- we choose a random nonce $r$
- And share its public nonce $R = rG$ together
- The signature becomes $(s, R)$ where $s = r + ke$
:::<!-- element style="font-size: 0.7em" -->

--

::: title
### ECDSA
:::

- public key & private key: $P = pG$
- Choose a random nonce $k$
- Its public nonce: $r = (kG).x$
- $s = {1 \over k}(e + p\cdot r)$
- Signature: $(r, s)$
- Verification: ${e \over s}G + {r \over s}P = ({e + rp \over s})G = kG$

---

### Pairing Based Cryptography

---

Q: How can Carl make sure that the shared secret key is computed correctly?

***
- Alice's key pair: $(a, g^a)$
- Bob's key pair: $(b, g^b)$
- Shared Secret Key: $g^{ab}$

note:
The answer is using the bi-linear mapping function

---
::: title
Bi-lineaer Mapping by [[Pairing]] Functions
:::


![](https://docs.google.com/drawings/d/e/2PACX-1vQhVd_tBcQz8So0Qjo93tz9IuPK6Bqd_9tQgVVyzTCQJT1K1Uv2dZz9YRqWPMXPwJau1NgCaL7LDfJ_/pub?w=891&amp;h=520)<!-- element style="width: 100%; margin-top: 100px;" -->

note:
- Imagine what characteristics this function should satisfy to verify Carl's question?
- $|\mathbb{G}_1| = |\mathbb{G}_{2}| = |\mathbb{G}_{T}|$ 
- Commutativity
- $e(g_1^{a}, g_2^{b}) = g_T^{ab}$


--

$$
e : \mathbb{G}_1 \times \mathbb{G}_2 \to \mathbb{G}_T
$$
***
- $e(g_1^{a}, g_2^{b}) = g_T^{ab}$
- bilinearity: $e([a]_{1}, [b]_{2}) \cdot e([a]_{1}, [c]_{2}) = e([a]_{1}, [b + c])$
	- $[a]_{1} = g_{1}^a$
	- $[b]_{2} = g_{2}^b$
	- $e(P_{1} + P_{2}, Q) = e(P_{1}, Q) \cdot e(P_{2}, Q)$
	- $e(P, Q_{1} + Q_{2}) = e(P, Q_{1}) \cdot e(P, Q_{2})$
- Non-degenerate:<br>
	$$\forall P \in \mathbb{G}_{1}, Q \in \mathbb{G}_{2}$$<br>$$e(P, Q) \neq 1$$


--

::: title
Pairing Functions
:::

1. Weil Pairing
2. Tate Pairing
3. The Optimal Ate Pairing $\rightarrow$ the most efficient

note:
https://eprint.iacr.org/2008/096
--

::: title
Pairing friendly curves
:::

1. [[BN(Barreto-Naehrig) Curves]]
	: The elliptic curve $E$ has an equation of the form $E: y^{2} = x^{3} + b$, where $b$ is a primitive element of the multiplicative group $\mathbb{F}_p^{*}$ of order $p - 1$.
1. [[BLS(Barreto-Lynn-Scott) Curves]]

note:
https://www.ietf.org/archive/id/draft-irtf-cfrg-pairing-friendly-curves-08.html#name-barreto-lynn-scott-curves

--

::: title
(optional) optimal Ate Pairing over BN Curve
:::

1.  $E: y^{2} = x^{3} + b$ <br>($b$ is a primitive element, a.k.a. a generator of the multiplicative group $\mathbb{F}_p^*$)
2. $E':  y^{2} = x^{3} + b'$ over $\mathbb{F}_{p^{2}}$(Twisted curve of $E$ which is its isomorphism)
	- M-type: $b' = b \cdot m$
	- D-type: $b' = b/m$

--

::: title
(optional) optimal Ate Pairing over BN Curve
:::

3. Miller Loop: computationally intensive part of the pairing operation. Doing double&add in $\mathbb{G}_2$ group.
4. Final Exponentiation: raise the miller loop result $f$ to the power of $(p^{12} - 1) / r$
5. Output $\mathsf{result} \in \mathbb{G}_{T}$ which is a multiplicative group of order $r$ in $\mathbb{F}_p^{12}$

note:
- For the final exponentiation
	- Cyclotomic Exponentiation
	- Decompose the exponent
	- Frobenius Automorphisms
	- $f^{\phi} = f^{p^{6}}\cdot f^{-p^{3}} \cdot f$

--
::: title
Security of Pairing Based Cryptography
:::

- ECDLP in $\mathbb{G_{1}}$, $\mathbb{G_{2}}$
- FFDLP in $\mathbb{G}_{T}$

- BLS12_381(Ethereum/Zcash): 128-bit security
- BN462: 128-bit security

---

::: title
Pairing Applications
:::
1. BLS signature
2. Polynomial Commitment (KZG commitment)

---

::: title
BLS Signature
:::

::: left
Setup

Private key: $x \in [q - 1]$ where $q = |\mathbb{G}|$<br>
Public key : $Q = [x]_2 = g_2^x$
***
Signing

$h = H(m) \in \mathbb{G}_1$
$\sigma = h^x$

***
Verification

$e(\sigma, G) \stackrel{?}{=} e(H(m), Q)$
:::<!-- element style="font-size: 0.7em; width: 100%"-->


--

::: title
Aggregation of BLS signatures
:::

::: left
Individual Signing

$\sigma_i = x_i \cdot H(m_i)$
***
Aggregation

$\sigma_{\text{agg}} = \prod_{i=1}^n \sigma_i$
***
Verification

$e(\sigma_{\text{agg}}, G) \stackrel{?}{=} \prod_{i=1}^n e(H(m_i), Q_i)$
:::<!-- element style="font-size: 0.7em; width: 100%"-->

---


KZG Polynomial Commitment Scheme

--

::: title
KZG Polynomial Commitment Scheme
:::

::: left
1. Prover $\mathcal{P}$ wants to prove that $\mathcal{P}$ has a polynomial $f(X) \in \mathbb{F}_{< d}[X]$ and $f(x_{1}) = y_{1}$
2. Verifier $\mathcal{V}$ picks a random number $r$ and then compute the set of powers of $r$ on the elliptic curve point groups
	$\{g, g^{r}, g^{r^{2}}, g^{r^{3}}, \dots, g^{r^{d-1}}\}$
3. Then, verifier requests $\mathcal{P}$ to compute $g^{f(r)}$ using the set of points.
4. Also, request $\mathcal{P}$ the quotient polynomial $g^{q(r)}$ s.t. <br>
	$f(X) - y_{1} = (X - x_{1})q(X)$
5. Then, compute $f(r) - y_{1} \stackrel{?}{=} (r - x_{1})\cdot q(r)$ using pairings.
 
:::<!-- element style="font-size: 0.7em; width: 100%;" -->

--
- this protocol is called the **[[public coin protocol]]** & the random oracle model
- here $f(r)$ is the **random oracle**
- By the [[Schwartz-Zippel Lemma]] we can assure that
	$\mathrm{Pr}[f(r) - y_{1} = (r - x_{1})\cdot q(r)] \leq \frac{d}{|\mathbb{F}|}$  by a random chance

--

::: title
Interactive Proof using a random oracle model
:::

```mermaid

sequenceDiagram
    participant Prover
    participant Verifier
    
    Prover->>Verifier: I have a polynomial f
    Note over Verifier: Picks a random number r
    Verifier->>Prover: Computes powers of r: {g, g^r, g^r², g^r³, ..., g^r^(d-1)}
    Prover->>Verifier: Provide an oracle g^f(r)
    Prover->>Verifier: Here're x1 and y1 and y1 = f(x1)
    Verifier->>Prover: Query proof using the quotient polynomial q(X) such that f(X) - y1 = (X-x1)q(X)
    Prover->>Verifier: Here is the oracle proof g^q(r)
    Note over Verifier: Accepts or Rejects by checking pairings w/ Schwartz Zippel Lemma
```
<!-- element style="width: 100%; margin-top: 100px;" -->

--

::: title
interactive proofs
:::
- completeness: $\mathcal{P}$ should be able to create a proof
- soundness: The probability of $\mathcal{V}$ accepts a wrong proof should be negligible

--
::: title
Schwartz-Zippel Lemma
:::

Let $\mathbb{F}$ be a field, and let $f(x_{1}, x_{2}, \ldots, x_{n})$ be a non-zero polynomial of total degree $d$ over $\mathbb{F}$. If we choose $(r_{1}, r_{2}, \ldots, r_{n})$ independently and uniformly at random from a finite set $\mathbb{F}$ of  then the probability that $f(r_{1}, r_{2}, \ldots, r_{n}) = 0$ is at most $\frac{d}{|F|}$.

--

::: title
An interactive proof to a non-interactive proof
:::

- Alice knows $a, g^a$
- Bob knows $b, g^b$
- Carl knows $c, g^c$
- What if the random $r = abc$ and if Alice, Bob, and Carl doesn't share the value with each other?

--

- This is called **[[Fiat-Shamir Heuristics]]** that transforms the interactive proof model using the random oracle access to a non-interactive proof model
- Also the process to prepare the uniformly chosen structured reference string is called **Trusted Setup**

note: Uniformly Selected: selected from a uniform distribution with random selection and unpredictability and statistically independently.

---

QnA