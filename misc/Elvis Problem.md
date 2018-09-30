```mermaid
graph LR
E(Elvis)
T(Twin)
NT(No Twin)
E--1.13%---T
E--98.87%---NT
F(Fraternal)
I(Identical)
T--70.80%---F
T--29.20%---I
null(...)
NT--0%---null
B1(Boy)
G1(Girl)
B2(Boy)
G2(Girl)
F--50%---B1
F--50%---G1
I--100%---B2
I--0%---G2
```

```
P(twin) & P(frat) = 1/125 = 0.8%
P(twin) & P(I) = 1/300 = 0.33%
P(twin) = 0.8% + 0.33% = 1.13% -> P(not twin) = 98.87%

P(frat|twin) = 0.8%/1.13%
P(I|twin) = 0.33%/1.13%

Elvis has twin brother. ->boy
P(boy) = (70.80% * 50%) + (29.20% * 100%) = 64.6%

Calculate P(I|boy) = P(boy|I).P(I)/P(boy)
= (100% * 29.20%)/64.6% = 45.20%

```

