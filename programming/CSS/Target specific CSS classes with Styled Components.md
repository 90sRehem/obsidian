#react #css #styled-components #programming 

![[Pasted image 20221226151317.png]]

solution:
```javascript
const Wrapper = styled.div`
  &.react-datepicker {
    color: blue;
  }
`

<Wrapper>
  <Datepicker />
</Wrapper>
```