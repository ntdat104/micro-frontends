# Micro frontends

## 1. Giới thiệu về mô hình Micro frontends

Hiện nay, các ứng dụng Single Page Apps (SPAs) cực kỳ phổ biến, chúng có nhiều tính năng và cũng rất phức tạp và thường được kết hợp với kiến trúc Microservices ở tầng backend. Sau một thời gian phát triển, các ứng dụng SPAs này trở nên cồng kềnh, và khó hơn cho việc maintain và chúng được gọi là Frontend Monolith.

Trong những năm trở lại đây, việc áp dụng những concepts từ Microservices vào các ứng dụng Frontend được nhắc đến khá thường xuyên. Ý tưởng của Micro Frontends đó là sẽ phân tách các ứng dụng này thành các phần kết hợp của các tính năng, mỗi tính năng có thể được phát triển bới một team độc lập.

![](https://micro-frontends.org/ressources/diagrams/organisational/monolith-frontback-microservices.png)

Các mô hình phát triển phần mềm từ trước khi có sự ra đời của Micro frontends.
Nguồn: https://micro-frontends.org/

**The Monolith**: một team phát triển toàn bộ các thành phần của sản phẩm từ Database, Backend, Frontend ⇒ Sẽ gặp một vấn đề khi sản phẩm lớn lên như sau:

- Không phải thành viên nào trong team cũng có thể làm được cả frontend và backend (Làm gì có một Junior developer có thể sáng code React và chiều code Golang một cách đỉnh cao được)

- Sản phẩm lớn, khối lượng kiến thức nhiều sẽ có khó thành viên nào nắm và hiểu sau được cả về frontend và backend

**Front & Back**: để khắc phục vấn đề trên ta có thể tách ra thành 2 team phát triển frontend và backend độc lập. Tuy nhiên đối với các sản phẩm lớn như một sàn thương mại điện tử (Tiki, Shopee,…) cần hỗ trợ các chức năng như sản phẩm, đặt hàng, thanh toán, tìm kiếm,… thì khối lượng công việc và kiến thức sẽ rất lớn và chúng ta cần phải áp dụng microservice để giải quyết các bài toán này.

**Microservices**: như đã nói ở trên, chúng ta chia nhỏ các chức năng thành các dịch vụ riêng để thuận tiện cho quá trình phát triển. Tuy nhiên việc phần chia các dịch vụ này chỉ ở phần backend cho nên phía frontend vẫn phải phát triển chung các chức năng với nhau ở một bộ source code.

![](https://micro-frontends.org/ressources/diagrams/organisational/verticals-headline.png)

**Mô hình Micro frontends**: mỗi team sẽ phát triển các sản phẩm độc lập (từ Database, Backend đến Frontend). Sau đó tích hợp các sản phẩm độc lập này lại với nhau thành một sản phẩm chung.
Nguồn: https://micro-frontends.org/

## 2. Khi nào ta nên nghĩ tới việc sử dụng Micro Frontends?

Một số trường hợp sau đây, chúng ta có thể sử dụng Micro Frontends:

- Một sản phẩm có nhiều module chức năng và bạn muốn nhiều team có thể phát triển cùng lúc

- Có thể bạn sẽ muốn phát triển một progressive hoặc responsive web application nhưng bạn gặp khó khăn trong việc tích hợp vào source code hiện tại của mình

- Có thể bạn muốn sử dụng một thư viện mới để tăng tốc quá trình phát triển sản phẩm của mình (vd: trước đó sử dụng Angularjs (1.x) để phát triển và hiện tại muốn sử dụng ReactJS để phát triển)

- Bạn muốn sử dụng một thư viện mới để hỗ trợ cho các chức năng sản phẩm, như sử dụng Webpack 5.x nhưng project hiện tại đang sử dụng Webpack 3.x và khó có thể nâng cấp lên Webpack 5.x được vì có khá nhiều dependence bị ảnh hưởng.

- Có thể bạn muốn tăng tốc quá trình phát triển sản phẩm bằng cách nhiều team khác nhau tham gia vào phát triển một sản phẩm cùng lúc bằng việc tách ra nhiều module và phát triển độc lập.

- Và rất nhiều lý do khác…

## 3. Một số ưu điểm và nhược điểm của Micro Frontends

![](https://micro-frontends.tuando.net/images/advantages.png)

#### Ưu điểm:

- Tách biệt các module chức năng thành nhiều phần source code riêng biệt. Từ đó giảm các dependencies ở mỗi project, lượng code sẽ ít hơn, giúp cho quá trình build deploy nhanh hơn và các file js bundle cũng sẽ nhẹ hơn

- Có khả năng mở rộng một cách dễ dàng bằng cách nhiều team cùng tham gia.

- Có thể sử dụng các thư viện, framework khác nhau (React, Angular) để phát triển các module khác nhau của một dự án.

- Có khả năng cập nhật, nâng cấp thư viện hoặc phát triển lại một phần nào đó của dự án.

- Dễ dàng kiểm thử (testing) các chức năng một cách độc lập.

#### Nhược điểm:

- Chia nhỏ các dự án sẽ dẫn tới trùng lập các dependencies hoặc source code

- Nhiều team phát triển nên khó trong việc quản lý source code nếu không có quy định chung rõ ràng từ ban đầu.

## 4. Một số phương pháp triển khai Micro Frontends

### Build-time integration

**Build-time integration** là việc coi các ứng dụng như một package và ứng dụng chính sẽ thêm các ứng dụng con như một thư viện như sau:

```json
{
  "name": "@micro-frontends/container",
  "version": "1.0.0",
  "description": "Micro frontends demo",
  "dependencies": {
    "@micro-frontends/products": "^1.2.3",
    "@micro-frontends/checkout": "^4.5.6",
    "@micro-frontends/user-profile": "^7.8.9"
  }
}
```

Cách tiếp cận này có một số hạn chế như:

- Chúng ta sẽ phải re-compile (bundle) các ứng dụng chính và release lại mỗi khi các ứng dụng con có thay đổi (release version mới từ 0.0.1 ⇒ 0.02)

- Không có sự đồng bộ chức năng giữa các ứng dụng chính nếu chúng ta bỏ xót quá trình đồng bộ version của ứng dụng con (Cũng có thể là một điểm lợi nếu chúng ta không muốn nâng cấp chức năng ở một trang nào đó)

- Phụ thuộc các dependencies với nhau
  - Nếu project `@micro-frontends/container` sử dụng React và `@micro-frontends/products` cũng sử dụng React thì sẽ bị trùng lập thư viện và tăng dung lượng khi tải trang web
  - Nếu project `@micro-frontends/container` sử dụng React và `@micro-frontends/products` sử dụng chung React với project chính thì sẽ bị phụ thuộc vào version của project chính.

### Run-time integration via iframes

```html
<html>
  <head>
    <title>Micro frontends</title>
  </head>
  <body>
    <h1>Welcome to Micro frontends</h1>

    <iframe id="micro-frontend-container"></iframe>

    <script type="text/javascript">
      const microFrontendsByRoute = {
        "/": "https://micro-frontends.tuando.net/demo/react-example",
        "/products":
          "https://micro-frontends.tuando.net/demo/react-example/products",
      };

      const iframe = document.getElementById("micro-frontend-container");
      iframe.src = microFrontendsByRoute[window.location.pathname];
    </script>
  </body>
</html>
```

Mỗi lần thay đổi url từ / sang /products phần nội dụng của trang sẽ được tải lại bởi một nội dung từ domain khác, trong ví dụ là https://micro-frontends.tuando.net/demo/react-example/products.

Demo chi tiết: https://micro-frontends.tuando.net/docs/example/iframes/

#### Ưu điểm:

- Không bị ảnh hưởng bởi styles (CSS) giữa các trang chính và trang trong iframe

#### Hạn chế:

- Phải tải lại toàn bộ trang khi thay đổi đường dẫn
- Khó khăn trong việc giao tiếp giữa các chức năng

### Run-time integration via JavaScript

Các tiếp cận này là việc chúng ta khai báo các global function hỗ trợ render các chức năng ở dự án con. Sau đó ở dự án chính ta sẽ gắn các script bundle file của các dự án con, tiếp theo cần hiện thị chức năng nào thì chỉ việc gọi chức năng đó thôi.

```jsx
import React from "react";
import ReactDOM from "react-dom";
import App from "./App";

window.renderProducts = (containerId, history) => {
  ReactDOM.render(
    <App history={history} />,
    document.getElementById(containerId)
  );
};
```

Chi tiết demo: https://micro-frontends.tuando.net/docs/example/run-time-integration/

### 4.4 Run-time integration via Web Components

Cách tiếp cận này cho phép chúng ta khai báo một HTML Custom Element, ví dụ như ta khai báo một HTML Custom Element `<web-components-products></web-components-products>` thì chỗ nào muốn sử dụng ta chỉ cần chèn đoạn mã `<web-components-products></web-components-products>` là có thể sử dụng được rồi.

Chi tiết demo: https://micro-frontends.tuando.net/docs/example/web-components/

#### Ưu điểm:

- Không bị phụ thuộc dependencies giữa các dự án với nhau (ví dụ: khác version React giữa các dự án)
- Vì cho phép tạo một HTML Custom Element nên ta có thể gắn thẻ HTML Custom này vào bất cứ đoạn mã HTML nào, không quan trọng dự án đó đang sử dụng frontend framework nào
- Hỗ trợ Shadow DOM: cho phép style css độc lập, không ảnh hưởng css giữa các dự án với nhau
- Có thể phát triển theo hướng package (publish lên một registry) mà không cần phải có domain host cho dự án vì vậy đơn giản trong việc quản lý các version release.

#### Hạn chế:

- Không thể chia sẻ tài nguyên giữa các dự án với nhau (ví dụ: sử dụng chung thư viện React)

### 4.5 Module Federation Webpack 5

**Module Federation** là một tính năng mới của Webpack 5. Nó cho phép chúng ta cấu hình để một ứng dụng có thể dynamic load code từ một ứng dụng khác.

Hiểu đơn giản là chúng ta có 2 ứng dụng được phát triển độc lập A và B, ứng dụng B là một phần nhỏ chức năng của ứng dụng A. Module Federation sẽ cho phép ta nhúng ứng dụng B và ứng dụng A và chia sẻ tài nguyên giữa chúng.

Chi tiết các bạn tham khảo tài liệu tại [Module Federation](https://webpack.js.org/concepts/module-federation/) và các ví dụ tại [Module Federation Examples](https://github.com/module-federation/module-federation-examples)

Chi tiết demo:

- [Web components trong React](https://micro-frontends.tuando.net/docs/example/react-example/)
- [Web components trong React kết hợp Redux](https://micro-frontends.tuando.net/docs/example/react-redux/)

#### Ưu điểm:

- Có thể chia sẻ tài nguyên giữa các dự án. Ví dụ dự án A sử dụng React 16.x và dự án B cũng sử dụng React 16.x thì khi tải module B sẽ không cần phải tải thêm React một lần nữa, nếu 2 version khác nhau thì nó sẽ tự động tải thêm version React còn thiếu.
- Giao tiếp giữa các dự án một cách đơn giản, có thể sử dụng chung một Redux store giữa các dự án với nhau

#### Hạn chế:

- Các dự án phải sử dụng Module Federation của Webpack 5.x
- Buộc phải các dự án phải có các static domain để tải các bundle file tương ứng. Vì các chức năng Module Federation chỉ hỗ trợ cấu hình tải các file từ một remote url

**_Tài liệu tham khảo:_**

- https://martinfowler.com/articles/micro-frontends.html
- https://micro-frontends.org/
