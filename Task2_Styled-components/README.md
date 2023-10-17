

# STYLED-COMPONENTS



1. **Installation**  
	```bash
    npm install styled-components
    ```
	
	* Về cơ bản styled-components là một dạng viết CSS trực tiếp trong file tsx <==> như tsx viết html trong ts
	CSS-in-TS
	
	* Extensiton : styled-components-snippets ==> dùng cho tiện tren vscode
		scp,
	
	> Link youtube tổng hợp của kênh evondev:
	[Styled-components](https://www.youtube.com/playlist?list=PLmnsJI3O-fYskJJ2nK1kGPXPzCYOTJAOb)
	
	* Một số import ghay dùng
	
		import styled from 'styled-components';  
		import { createGlobalStyle } from 'styled-components';  
		import { css } from 'styled-components';  
		import { withTheme } from 'styled-components';  
2. **Getting Started**  
	Một số syntax cơ bản khi dùng styled-components:
	```
        const Tags = styled."tags html"`
			"CSS"`;
    ```
	vd1:		
```ts
            function Components() {
			<tags html>
				"text"
			</tags html>
			}
			
		vd1:
			const Title = styled.h1`
				  font-size: 1.5em;
				  text-align: center;
				  color: #BF4F74;
				`;

				const Wrapper = styled.section`
				  padding: 4em;
				  background: papayawhip;
				`;

				render(
				  <Wrapper>
					<Title>
					  Hello World!
					</Title>
				  </Wrapper>
				);
```
vd2:
```ts
            const Input = styled.input<{ $inputColor?: string; }>`
			  padding: 0.5em;
			  margin: 0.5em;
			  color: ${props => props.$inputColor || "#BF4F74"};
			  background: papayawhip;
			  border: none;
			  border-radius: 3px;
			`;

			
			render(
			  <div>
				<Input defaultValue="@probablyup" type="text" />
				<Input defaultValue="@geelen" type="text" $inputColor="rebeccapurple" />
			  </div>
					);
```
				
* Thẻ sẽ chứa tất cả các tính chất CSS dùng bên trên ==> thuận tiện cho xây dựng từng components riêng lẻ  
3. **Use props**  
```ts
      const Button = styled.button<{ $primary?: boolean; }>`
		  background: ${props => props.$primary ? "#BF4F74" : "white"};
		  color: ${props => props.$primary ? "white" : "#BF4F74"};

		  font-size: 1em;
		  margin: 1em;
		  padding: 0.25em 1em;
		  border: 2px solid #BF4F74;
		  border-radius: 3px;
		`;

		render(
		  <div>
			<Button>Normal</Button>
			<Button $primary>Primary</Button>
		  </div>
	);
```
* Một dạng sư dụng props khác 
```ts
    ${(props) ==> 
		props.secondary &&
		css`
		background: linear-gradient(86.88deg, #20e3b,#2cccff);
		`
	}
```
	
* Sử dụng props trong thẻ button có thể xử lý như một hàm có state css, có thể dùng trực tiếp arow function trong này
	
4. **Extending Styles**  
	* Tính kế thừa của styled-components dùng lại một styled và viết tiếp thuộc tính hoặc ghi đề lên giá trị cũ 
	
vd:
```ts
        const Button = styled.button`
		  color: #BF4F74;
		  font-size: 1em;
		  margin: 1em;
		  padding: 0.25em 1em;
		  border: 2px solid #BF4F74;
		  border-radius: 3px;
		`;

		const TomatoButton = styled(Button)`
		  color: tomato;
		  border-color: tomato;
		`;

		render(
		  <div>
			<Button>Normal Button</Button>
			<TomatoButton>Tomato Button</TomatoButton>
		  </div>
		);
```
		
* Như trên thấy TomatoButton có style được ghi đè lệnh color borser-color
		
* dùng as="new tag html " ==> sẽ thay đổi loại tags cho cái styped ấy 
		 
vd:
```ts
            render(
			  <div>
				<Button>Normal Button</Button>
				<Button as="a" href="#">Link with Button styles</Button>
				<TomatoButton as="a" href="#">Link with Tomato Button styles</TomatoButton>
			  </div>
					);
```
					
* Ví dụ về custom components 		
```ts
                    const Button = styled.button`
					  display: inline-block;
					  color: #BF4F74;
					  font-size: 1em;
					  margin: 1em;
					  padding: 0.25em 1em;
					  border: 2px solid #BF4F74;
					  border-radius: 3px;
					  display: block;
					`;

					const ReversedButton = props => <Button {...props} children={props.children.split('').reverse()} />

					render(
					  <div>
						<Button>Normal Button</Button>
						<Button as={ReversedButton}>Custom Button with Normal Button styles</Button>
					  </div>
					);
```
					
5. **Styling any component**
* style trực tiếp components đã có sẵn theo syntax
		
```
        const NewComponent = styled(Components)`
		"CSS"`
		`
```
6.	**Pseudoelements, pseudoselectors, and nesting**

* vd 1: cách dùng hover các thẻ có vị trí khác nhau cùng với className
	
```ts
                const Thing = styled.div.attrs((/* props */) => ({ tabIndex: 0 }))`
			  color: blue;

			  &:hover {
				color: red; // <Thing> when hovered
			  }

			  & ~ & {
				background: tomato; // là ae của nó nhưng không bên cạnh nó 
			  }

			  & + & {
				background: lime; // bên cạnh nó là anh em 
			  }

			  &.something {
				background: orange; // <Thing> tagged with an additional CSS class ".something"
			  }

			  .something-else & {
				border: 1px solid; // <Thing> inside another element labeled ".something-else"
			  }
			`

			render(
			  <React.Fragment>
				<Thing>Hello world!</Thing>
				<Thing>How ya doing?</Thing>
				<Thing className="something">The sun is shining...</Thing>
				<div>Pretty nice day today.</div>
				<Thing>Don't you think?</Thing>
				<div className="something-else">
				  <Thing>Splendid.</Thing>
				</div>
			  </React.Fragment>
			)
```
			
* && riêng một ký hiệu kép có một hành vi đặc biệt được gọi là "tăng mức độ ưu tiên"; điều này có thể hữu ích nếu bạn đang xử lý một môi trường CSS hỗn hợp và các thành phần có kiểu dáng, nơi có thể có các kiểu xung đột:
	
7. **.attrs constructor**

* thêm tính chất cho tags

```ts
const Input = styled.input.attrs<{ $size?: string; }>(props => ({
  // we can define static props
  type: "text",

  // or we can define dynamic ones
  $size: props.$size || "1em",
}))`
  color: #BF4F74;
  font-size: 1em;
  border: 2px solid #BF4F74;
  border-radius: 3px;

  /* here we use the dynamically computed prop */
  margin: ${props => props.$size};
  padding: ${props => props.$size};
`;

render(
  <div>
    <Input placeholder="A small text input" />
    <br />
    <Input placeholder="A bigger text input" $size="2em" />
  </div>
);
```

8. **Animations**

```ts
// Create the keyframes
const rotate = keyframes`
  from {
    transform: rotate(0deg);
  }

  to {
    transform: rotate(360deg);
  }
`;

// Here we create a component that will rotate everything we pass in over two seconds
const Rotate = styled.div`
  display: inline-block;
  animation: ${rotate} 2s linear infinite;
  padding: 2rem 1rem;
  font-size: 1.2rem;
`;

render(
  <Rotate>&lt; 💅🏾 &gt;</Rotate>
);
```

9. **Theme**

* lưu một số gt thêm trong mảng theme
	
```ts
    import {ThemeProbider} from "styled-components";
			const Button = styled.button`
			  font-size: 1em;
			  margin: 1em;
			  padding: 0.25em 1em;
			  border-radius: 3px;

			  /* them màu qua props theme.main */
			  color: ${props => props.theme.main};
			  border: 2px solid ${props => props.theme.main};
			`;

			// Tạo một object có chứa mảng thông tin màu 
			const theme = {
			  main: "mediumseagreen"
			};

			render(
			  <div>
				<Button theme={{ main: "royalblue" }}>Ad hoc theme</Button>
				<ThemeProvider theme={theme}>
				  <div>
					<Button>Themed</Button>
					<Button theme={{ main: "darkorange" }}>Overridden</Button>
				  </div>
				</ThemeProvider>
			  </div>
			);
```

* chú ý mảng này để thưc hiện nâng cao hơn
	
```ts
    import { withTheme } from 'styled-components'
	import { useContext } from 'react'
	import { ThemeContext } from 'styled-components'	
	import { useTheme } from 'styled-components'
```
	
	
10. **Tagged Template Literals**

vd1:
```ts
    const aVar = 'good';
		fn`this is a ${aVar} day`;
		fn(['this is a ', ' day'], aVar);
```
		
vd2:
```ts
    const Title = styled.h1<{ $upsideDown?: boolean; }>`
		  ${props => props.$upsideDown && 'transform: rotate(180deg);'}
		  text-align: center;
		`;
```
		
11. **Global styled** 		
 
* dùng để tạo một số CSS chung cho dự án 
vd1 :
```ts
        import { createGlobalStyle } from "styled-components";
		import { GlobalClass } from "./..."
		export const GlobalStyle = createGlobalStyle`
		@import url('....') // url font chữ tải trên google font 
		body {
			font-family: 'Poppins', sans-serif;
			};
		${GlobalClass} // đưa được file class vào trung trong file global
		`;
```
		
* ngoài ra có thể tạo thêm một file GlobalClass để đưa thêm một số class dùng trung ở global vào 
			
```ts
        import {css} from "styled-components";
		
		export const GlobalClasses = css`
		.demo{
			display: flex;
			justify-content: space-between;
			align-items: center;
		}

		`;
```
		
12. **cách sử dụng styled-components có 2 cách phổ biến**

	`c1`: từng tags(h1,div,a,img,...} sẽ có từng thẻ styled-components một có một styles  
	`c2`: dùng môt thẻ ngoài cùng rồi đặt tên className rồi sử dùng rất giống SCSS
	
	```
    import {css} from "styled-components";
    ```
	
	
	
	
# TỔNG KẾT: 
1. sử dụng styled-components dạng CSS in ts, ngoài ra cấu trúc có thể viết như SCSS
2. chú ý cách dùng theme, props, css,.attrs, components
3. cách dùng createGlobalStyle,Tagged Template Literals,Animations
		
