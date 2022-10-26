# Seaborn Introduction


## Distribution plots
```
# kind = 'hist' by default
sns.displot(df['Award_Amount'])

# Create a displot
sns.displot(df['Award_Amount'],kde=False,bins=20)

sns.displot(df['Award_Amount'],kind='kde',rug=True,fill=True)
```

## Regression analysis - bivariate

- `regplot` is lowlevel
- `lmplot` is highlevel and more powerful, supports faceting with `hue` and `col`

```
sns.regplot(x="insurance_losses", y="premiums", data=df)

sns.lmplot(x="insurance_losses", y="premiums", data=df)

sns.lmplot(data=df,x="insurance_losses",y="premiums",hue="Region")

sns.lmplot(data=df,x="insurance_losses",y="premiums",row="Region")
```