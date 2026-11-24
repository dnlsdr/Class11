Struktúra:
```
Sokszogek (absztrakt, üres)
├── Haromszog  implements KeruletTeruletSzamithato
└── Negyszog   implements KeruletSzamithato
    └── Teglalap  implements KeruletTeruletSzamithato
        └── Negyzet

Kor  implements KeruletTeruletSzamithato   (nem sokszög, ezért nem a Sokszogek leszármazottja)

KeruletTeruletSzamithato extends KeruletSzamithato

HibasSokszogException (extends Exception)
├── HibasHaromszogException
└── HibasNegyszogException
```
