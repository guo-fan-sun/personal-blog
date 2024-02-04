---
{"dg-publish":true,"permalink":"/个人博客/01-逆向抖音企业号 X-Bogus 和 _signature 参数/01-逆向抖音企业号 X-Bogus 和 _signature 参数/"}
---


- 开始时间: 2023-11-17
- 结束时间: 2023-12-11
## 注意事项
- 该接口在登录状态下逆向
- 接口逆向过程，需要先定位接口，并确定接口是否需要逆向
	- 使用浏览器打开网页, 测试用户正常访问的接口请求和响应结果
	- 查看请求头和请求参数中是否存在需要逆向的参数
		- 一般若存在参数为一组变化的无明确含义的字符串, 则该参数有需要逆向的可能
		- 若需逆向, 则需继续调试, 根据结果确定是否需要逆向
## 固定数据
- 请求的 url: `https://leads.cluerich.com/aweme/v1/saiyan/support/account/employee/reset/password`
## 查找逆向参数

- 确认逆向的接口
	- 清空浏览器日志, 以更好的定位接口
		- ![清空浏览器日志.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E6%B8%85%E7%A9%BA%E6%B5%8F%E8%A7%88%E5%99%A8%E6%97%A5%E5%BF%97.png)
		- 手动点击按钮发送请求
		- 查看浏览器请求，通过检索或其他方式定位接口
			- ![定位接口.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E5%AE%9A%E4%BD%8D%E6%8E%A5%E5%8F%A3.png)

- 编写 python 代码发送请求
	- 这里可以使用代码生成工具 , 快速生成格式化代码, 操作步骤如下
		- 点击指定的接口, 【右键】-【以 cURL(bash)格式复制】
			- ![复制 bash 格式 cURL.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E5%A4%8D%E5%88%B6%20bash%20%E6%A0%BC%E5%BC%8F%20cURL.png)
		- 打开[curl 转 python 工具](https://trumanwl.com/development/curl/python)
		- 复制代码保存为 python 文件 ![复制生成的代码.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E5%A4%8D%E5%88%B6%E7%94%9F%E6%88%90%E7%9A%84%E4%BB%A3%E7%A0%81.png)
- 保存代码, 运行, 查看是否能成功发送请求
	- 若能, 则有可能该接口并不需要逆向
		- 但不能排除参数中存在由时间戳生成的数据，只是过期时间较长，并不限于单次请求
		- 进一步检查可疑的参数, 将其修改或注释, 查看返回结果
			- 若正常返回, 则不需逆向
			- 若未正常返回，则进行下一步
	- 若不能，则需逐一测试。测试完毕后, 总结所有需要逆向的参数, 逐个查看参数生成逻辑
- 经观察和测试, 有以下参数需要逆向 `X-Bogus` 和 `_signature`
	- 经最终测试，这两个参数并未被检查，由于未能准确定位问题，在无关紧要的参数上浪费了太多时间

## 逆向参数 `X-Bogus`
- 使用请求的 url 添加 XHR 断点 `/aweme/v1/saiyan/support/account/employee/reset/password`
	- ![添加 XHR 断点.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E6%B7%BB%E5%8A%A0%20XHR%20%E6%96%AD%E7%82%B9.png)
- 手动发送请求, 查看`调用堆栈`
	- ![查看堆栈中变量.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E6%9F%A5%E7%9C%8B%E5%A0%86%E6%A0%88%E4%B8%AD%E5%8F%98%E9%87%8F.png)
	- ![展开变量.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E5%B1%95%E5%BC%80%E5%8F%98%E9%87%8F.png) 可以看到, `X-Bogus` 和 `_signature` 已被生成
- 跟栈查看
	- ![抖音企业号对更新密码接口进行跟栈.gif](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%E5%AF%B9%E6%9B%B4%E6%96%B0%E5%AF%86%E7%A0%81%E6%8E%A5%E5%8F%A3%E8%BF%9B%E8%A1%8C%E8%B7%9F%E6%A0%88.gif)
- 接下的栈因为异步调用的原因, 无法继续在作用域查看
	- ![抖音企业号对更新密码接口进行跟栈-02.gif](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%E5%AF%B9%E6%9B%B4%E6%96%B0%E5%AF%86%E7%A0%81%E6%8E%A5%E5%8F%A3%E8%BF%9B%E8%A1%8C%E8%B7%9F%E6%A0%88-02.gif)
- 通过对代码观察, 推测有几个栈只是发送请求, 不涉及参数加密
- 继续往下跟栈, 看到存在混淆的代码, 加密很可能在此处
	- ![定位到混淆 js.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E5%AE%9A%E4%BD%8D%E5%88%B0%E6%B7%B7%E6%B7%86%20js.png)
- 此处需要对混淆的代码进行反混淆
	- 使用浏览器插件 [v_jstool](https://github.com/cilame/v_jstools) 进行反混淆
		- [v_jstool 下载与使用说明](https://www.cnblogs.com/yoyo1216/p/17601370.html)
		- ![反混淆 webmssdk.gif](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E5%8F%8D%E6%B7%B7%E6%B7%86%20webmssdk.gif)
		- 复制反混淆之后代码保存到本地
- 将 `webmssdk.js` 保存为本地替换文件
	- 打开【浏览器开发者工具】，点击【替换】，选择替换文件夹
		- ![选择替换文件夹.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E9%80%89%E6%8B%A9%E6%9B%BF%E6%8D%A2%E6%96%87%E4%BB%B6%E5%A4%B9.png)
	- 点击【网页】，打开源文件，保存为本地替换
		- ![网页源文件保存为本地文件.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E7%BD%91%E9%A1%B5%E6%BA%90%E6%96%87%E4%BB%B6%E4%BF%9D%E5%AD%98%E4%B8%BA%E6%9C%AC%E5%9C%B0%E6%96%87%E4%BB%B6.png)
		- ![成功替换文件夹.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E6%88%90%E5%8A%9F%E6%9B%BF%E6%8D%A2%E6%96%87%E4%BB%B6%E5%A4%B9.png)
	- 
- 释放断点, 刷新网页, 重新手动发送请求
	- 注意, 之后每次更新本地替换文件后都需要刷新页面
- ![反混淆后 webmssdk 的代码.gif](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E5%8F%8D%E6%B7%B7%E6%B7%86%E5%90%8E%20webmssdk%20%E7%9A%84%E4%BB%A3%E7%A0%81.gif)
- 观察如上代码, 代码可读性降低, 猜测为加密源码
	- 源码
	```js
		function G(a, b, e, d, f, _, w, g) {
			null == _ && (_ = this);
			var p, y, m, S = [], A = 0;
			// 临时变量
			var tmp;
			var json_stringify = function(data){
				var cahce = [], text;
				text = JSON.stringify(
					data, 
					(key, val)=>{
						if(cahce.indexOf(val)!=-1){
							return undefined;
						}
						cahce.push(val);
						// if(Array.isArray(val) || typeof val=="string"){
						//     return val.slice(0, 100)
						// }
						return val;
					}
				);
				cahce = null;
				// return text?text.slice(0, 100):text
				return text?text:""
			};
			w && (p = w);
			var E, T, C = b, O = C + 2 * e;
			if (!g) {
				// console.log("webmssdk 9-1", json_stringify(f))
				for (;C < O; ) {
					var I = parseInt("" + a[C] + a[C + 1], 16);
					C += 2;
					var R = 3 & (E = 13 * I % 241);
					if (E >>= 2, R < 1) {
						R = 3 & E;
						if (E >>= 2, R > 2) {
							(R = E) < 1 ? S[++A] = null : R < 3 ? (p = S[A--], S[A] = S[A] >= p) : R < 12 && (S[++A] = void 0);
						} else if (R > 1) {
							if ((R = E) > 11) {
								throw S[A--];
							}
							if (R > 7) {
								for (p = S[A--], T = v(a, C), R = "", N = n.q[T][0]; N < n.q[T][1]; N++) {
									R += String.fromCharCode(o ^ n.p[N]);
									// console.log("webmssdk 3-101", String.fromCharCode(o ^ n.p[N]), "=", "fromCharCode(", o, "^", n.p[N], ")");
								}
								C += 4, S[A--][R] = p;
								// console.log("webmssdk 3-1", "S[A][R]=", p);
							}
						} else if (R > 0) {
							(R = E) > 8 ? (p = S[A--], S[A] = typeof p) : R > 4 ? (
								tmp = S[A-1],
								S[A -= 1] = S[A][S[A + 1]]
								// ,console.log("webmssdk 3-107", tmp, "=", S[A])
							) : R > 2 && (y = S[A--], 
							(R = S[A]).x === G ? R.y >= 1 ? (S[A] = z(a, R.c, R.l, [ y ], R.z, m, null, 1)
								// ,console.log("webmssdk 3-2", "S[A]=", S[A])
							) : (S[A] = z(a, R.c, R.l, [ y ], R.z, m, null, 0), 
							// console.log("webmssdk 3-3", "S[A]=", S[A]),
							R.y++) : (S[A] = R(y)
							// ,console.log("webmssdk 3-4", "S[A]=", S[A], "y=", y, "R=", R)
							));
						} else if ((R = E) > 14) {
							T = u(a, C), (P = function b() {
								var e = arguments;
								return b.y > 0 ? z(a, b.c, b.l, e, b.z, this, null, 0) : (b.y++, z(a, b.c, b.l, e, b.z, this, null, 0));
							}).c = C + 4, P.l = T - 2, P.x = G, P.y = 0, P.z = f, S[A] = P, C += 2 * T - 2;
							// console.log("webmssdk 3-5", "S[A]=", P);
						} else if (R > 12) {
							y = S[A--], m = S[A--], (R = S[A--]).x === G ? R.y >= 1 ? (S[++A] = z(a, R.c, R.l, y, R.z, m, null, 1)
							// ,console.log("webmssdk 3-6", "S[A]=", S[A])
							) : (S[++A] = z(a, R.c, R.l, y, R.z, m, null, 0), 
							// console.log("webmssdk 3-7", "S[A]=", S[A]),
							R.y++) : (S[++A] = R.apply(m, y)
							// , console.log("webmssdk 3-8", "S[A]=", S[A], "R=", R, "m=", m, "y=", y)
							)
							;
						} else if (R > 5) {
							p = S[A--], S[A] = S[A] != p;
						} else if (R > 3) {
							tmp = S[A-1];
							p = S[A--], S[A] = S[A] * p;
							// console.log("webmssdk 3-9", "S[A]=", S[A], "=", tmp, "*", p);
						} else if (R > -1) {
							// console.log("webmssdk 5-1", "S[A]=", S[A]);
							return [ 1, S[A--] ];
						}
					} else if (R < 2) {
						R = 3 & E;
						if (E >>= 2, R < 1) {
							if ((R = E) > 9) {} else if (R > 7) {
								tmp = S[A-1]
								p = S[A--], S[A] = S[A] & p;
								// console.log("webmssdk 3-10", "S[A]=", S[A], "=", tmp, "&", p);
							} else if (R > 5) {
								T = l(a, C), C += 2, S[A -= T] = 0 === T ? new S[A]() : c(S[A], t(S.slice(A + 1, A + T + 1)));
							} else if (R > 3) {
								T = u(a, C);
								try {
									if (i[r][2] = 1, 1 == (p = G(a, C + 4, T - 3, [], f, _, null, 0))[0]) {
										// console.log("webmssdk 5-2", "p=", p);
										return p;
									}
								} catch (b) {
									if (i[r] && i[r][1] && 1 == (p = G(a, i[r][1][0], i[r][1][1], [], f, _, b, 0))[0]) {
										// console.log("webmssdk 5-3", "p=", p);
										return p;
									}
								} finally {
									if (i[r] && i[r][0] && 1 == (p = G(a, i[r][0][0], i[r][0][1], [], f, _, null, 0))[0]) {
										// console.log("webmssdk 5-4", "p=", p);
										return p;
									}
									i[r] = 0, r--;
								}
								C += 2 * T - 2;
							}
						} else if (R < 2) {
							if ((R = E) > 12) {
								S[++A] = x(a, C), C += 2;
								// console.log("webmssdk 3-11", "S[A]=", S[A]);
							} else if (R > 10) {
								tmp = S[A-1];
								p = S[A--], S[A] = S[A] << p;
								// console.log("webmssdk 3-12", "S[A]=", S[A], "=", tmp, "<<", p);
							} else if (R > 8) {
								for (T = v(a, C), R = "", N = n.q[T][0]; N < n.q[T][1]; N++) {
									R += String.fromCharCode(o ^ n.p[N]);
									// console.log("webmssdk 3-102", String.fromCharCode(o ^ n.p[N]), "=", "fromCharCode(", o, "^", n.p[N], ")");
								}
								tmp = S[A];
								C += 4, S[A] = S[A][R];
								// console.log("webmssdk 3-103", tmp, "=", S[A]);
							} else {
								R > 6 && (y = S[A--], p = delete S[A--][y]);
							}
						} else if (R < 3) {
							(R = E) < 2 ? S[++A] = p : R < 11 ? (p = S[A -= 2][S[A + 1]] = S[A + 2], A--) : R < 13 && (p = S[A], 
							S[++A] = p
							// ,console.log("webmssdk 3-13", "S[A]=", p)
							);
						} else if ((R = E) < 1) {
							S[A] = !S[A];
						} else if (R < 3) {
							if ((T = u(a, C)) < 0) {
								g = 1, q(a, b, 2 * e), C += 2 * T - 2;
								break;
							}
							C += 2 * T - 2;
						} else {
							R < 5 ? (
								tmp = S[A-1],
								p = S[A--], S[A] = S[A] / p
							// ,console.log("webmssdk 6-1", "S[A]=", S[A], "=", tmp, "/", p)
							) : R < 7 ? (p = S[A--], S[A] = S[A] !== p) : R < 14 && (S[++A] = _
								// ,console.log("webmssdk 3-14", "S[A]=", S[A], "=", _)
								);
						}
					} else if (R < 3) {
						R = 3 & E;
						if (E >>= 2, R > 2) {
							(R = E) > 7 ? (
								tmp = S[A-1],
								p = S[A--], S[A] = S[A] | p
								// ,console.log("webmssdk 3-15", "S[A]=", S[A], "=", tmp, "|", p)
								) : R > 5 ? (T = l(a, C), C += 2, S[++A] = f["$" + T]
								// ,console.log("webmssdk 3-16", "S[A]=", S[A])
								) : R > 3 && (T = u(a, C), 
							i[r][0] && !i[r][2] ? i[r][1] = [ C + 4, T - 3 ] : i[r++] = [ 0, [ C + 4, T - 3 ], 0 ], 
							C += 2 * T - 2);
						} else if (R > 1) {
							if ((R = E) > 13) {
								S[++A] = !1;
							} else if (R > 6) {
								p = S[A--], S[A] = S[A] instanceof p;
							} else if (R > 4) {
								tmp = S[A-1];
								p = S[A--], S[A] = S[A] % p;
								// console.log("webmssdk 3-17", "S[A]=", S[A], "=", tmp, "%", p);
							} else if (R > 2) {
								if (S[A--]) {
									C += 4;
								} else {
									if ((T = u(a, C)) < 0) {
										g = 1, q(a, b, 2 * e), C += 2 * T - 2;
										break;
									}
									C += 2 * T - 2;
								}
							} else if (R > 0) {
								for (T = v(a, C), p = "", N = n.q[T][0]; N < n.q[T][1]; N++) {
									p += String.fromCharCode(o ^ n.p[N]);
									// console.log("webmssdk 3-104", String.fromCharCode(o ^ n.p[N]), "=", "fromCharCode(", o, "^", n.p[N], ")");
								}
								S[++A] = p, C += 4;
								// console.log("webmssdk 3-18", "S[A]=", p);
							}
						} else {
							R > 0 ? (R = E) > 3 ? (p = S[A--], S[A] = S[A] == p) : R > 1 ? (
								tmp = S[A-1],
								p = S[A--], S[A] = S[A] + p
								// ,console.log("webmssdk 3-19", "S[A]=", S[A], "=", tmp, "+", p)
								) : R > -1 && (
									S[++A] = h
									// , console.log("webmssdk 3-20", "S[A]=", h)
								) : (R = E) < 2 ? (p = S[A--], 
							S[A] = S[A] > p) : R < 9 ? (T = v(a, C), C += 4, y = A + 1, S[A -= T - 1] = T ? S.slice(A, y) : []) : R < 11 ? (T = l(a, C), 
							C += 2, p = S[A--], f[T] = p) : R < 13 ? (
								tmp = S[A-1],
								p = S[A--], S[A] = S[A] >> p
								// ,console.log("webmssdk 3-21", "S[A]=", S[A], "=", tmp, ">>", p)
								) : R < 15 && (S[++A] = u(a, C), 
							C += 4
							// ,console.log("webmssdk 3-22", "S[A]=", S[A])
							);
						}
					} else {
						R = 3 & E;
						if (E >>= 2, R < 1) {
							if ((R = E) > 13) {
								p = S[A], S[A] = S[A - 1], S[A - 1] = p;
							} else if (R > 4) {
								p = S[A--], S[A] = S[A] === p;
							} else if (R > 2) {
								tmp = S[A-1];
								p = S[A--], S[A] = S[A] - p;
								// console.log("webmssdk 3-23", "S[A]=", S[A], "=", tmp, "-", p);
							} else if (R > 0) {
								for (T = v(a, C), R = "", N = n.q[T][0]; N < n.q[T][1]; N++) {
									R += String.fromCharCode(o ^ n.p[N]);
									// console.log("webmssdk 3-105", String.fromCharCode(o ^ n.p[N]), "=", "fromCharCode(", o, "^", n.p[N], ")");
								}
								R = +R, C += 4, S[++A] = R;
								// console.log("webmssdk 3-106", "S[A]=", R)
							}
						} else if (R < 2) {
							if ((R = E) < 3) {
								var M = 0, U = S[A].length, j = S[A];
								S[++A] = function() {
									var a = M < U;
									if (a) {
										var b = j[M++];
										S[++A] = b;
										// console.log("webmssdk 3-24", "S[A]=", b);
									}
									S[++A] = a;
									// console.log("webmssdk 3-25", "S[A]=", a);
								};
							} else {
								R < 5 ? (T = l(a, C), C += 2, p = f[T], S[++A] = p
								// ,console.log("webmssdk 3-26", "S[A]=", p)
								) : R < 7 ? S[A] = ++S[A] : R < 9 && (p = S[A--], 
								S[A] = S[A] in p);
							}
						} else {
							R < 3 ? (R = E) > 10 ? (T = u(a, C), i[++r] = [ [ C + 4, T - 3 ], 0, 0 ], C += 2 * T - 2) : R > 8 ? (
								tmp = S[A-1],
								p = S[A--], 
							S[A] = S[A] ^ p
							// ,console.log("webmssdk 3-27", "S[A]=", S[A], "=", tmp, "^", p)
							) : R > 6 && (p = S[A--]) : (R = E) > 13 ? (S[++A] = s(a, C), C += 8
							// ,console.log("webmssdk 3-28", "S[A]=", S[A])
							) : R > 11 ? (
								tmp=S[A-1],
								p = S[A--], 
							S[A] = S[A] >>> p
							// ,console.log("webmssdk 3-29", "S[A]=", S[A], "=", tmp, ">>>", p)
							) : R > 9 ? S[++A] = !0 : R > 7 ? (
								tmp = S[A],
								T = l(a, C), C += 2, S[A] = S[A][T]
								// ,console.log("webmssdk 3-30", tmp, "=", S[A])
							) : R > 0 && (p = S[A--], 
							S[A] = S[A] < p);
						}
					}
				}
			}
			if (g) {
				for (;C < O; ) {
					I = F[C], C += 2, R = 3 & (E = 13 * I % 241);
					if (E >>= 2, R > 2) {
						R = 3 & E;
						if (E >>= 2, R > 2) {
							(R = E) < 2 ? (p = S[A--], S[A] = S[A] < p) : R < 9 ? (
								tmp=S[A],
								T = H[C], C += 2, S[A] = S[A][T]
								// ,console.log("webmssdk 2-101", tmp, "=", S[A])
								) : R < 11 ? S[++A] = !0 : R < 13 ? (p = S[A--],
							tmp = S[A], 
							S[A] = S[A] >>> p
							// ,console.log("webmssdk 2-1", "S[A]=", S[A], "=", tmp, ">>>", p)
							) : R < 15 && (S[++A] = H[C], C += 8
								// , console.log("webmssdk 2-2", "S[A]=", S[A])
								);
						} else if (R > 1) {
							(R = E) < 6 || (R < 8 ? p = S[A--] : R < 10 ? (p = S[A--], 
								tmp = S[A],
								S[A] = S[A] ^ p
								// , console.log("webmssdk 2-3", "S[A]=", S[A], "=", tmp, "^", p)
								) : R < 12 && (T = H[C], 
							i[++r] = [ [ C + 4, T - 3 ], 0, 0 ], C += 2 * T - 2));
						} else if (R > 0) {
							(R = E) > 7 ? (p = S[A--], S[A] = S[A] in p) : R > 5 ? S[A] = ++S[A] : R > 3 ? (T = H[C], 
							C += 2, p = f[T], S[++A] = p
							// , console.log("webmssdk 2-4", "S[A]=",p)
							) : R > 1 && (M = 0, U = S[A].length, j = S[A], S[++A] = function() {
								var a = M < U;
								if (a) {
									var b = j[M++];
									S[++A] = b;
									// console.log("webmssdk 2-5", "S[A]=", b);
								}
								S[++A] = a;
								// console.log("webmssdk 2-6", "S[A]=", a)
							});
						} else if ((R = E) > 13) {
							p = S[A], S[A] = S[A - 1], S[A - 1] = p;
						} else if (R > 4) {
							p = S[A--], S[A] = S[A] === p;
						} else if (R > 2) {
							tmp = S[A-1];
							p = S[A--], S[A] = S[A] - p;
							// console.log("webmssdk 2-7", S[A], "=", tmp, "-", p)
						} else if (R > 0) {
							for (T = H[C], R = "", N = n.q[T][0]; N < n.q[T][1]; N++) {
								R += String.fromCharCode(o ^ n.p[N]);
								// console.log("webmssdk 2-101", String.fromCharCode(o ^ n.p[N]), "=", o, "^", n.p[N])
							}
							R = +R, C += 4, S[++A] = R;
							// console.log("webmssdk 2-8", "S[A]=", R);
						}
					} else if (R > 1) {
						R = 3 & E;
						if (E >>= 2, R > 2) {
							(R = E) > 7 ? (
								tmp = S[A-1],
								p = S[A--], S[A] = S[A] | p
								// , console.log("webmssdk 2-9", S[A], "=" , tmp, "|", p)
								) : R > 5 ? (T = H[C], C += 2, S[++A] = f["$" + T]
									// ,console.log("webmssdk 2-102", "S[A]=", S[A])
								) : R > 3 && (T = H[C], 
							i[r][0] && !i[r][2] ? i[r][1] = [ C + 4, T - 3 ] : i[r++] = [ 0, [ C + 4, T - 3 ], 0 ], 
							C += 2 * T - 2);
						} else if (R > 1) {
							if ((R = E) > 13) {
								S[++A] = !1;
							} else if (R > 6) {
								p = S[A--], S[A] = S[A] instanceof p;
							} else if (R > 4) {
								tmp = S[A-1]
								p = S[A--], S[A] = S[A] % p;
								// console.log("webmssdk 2-10", S[A], "=", tmp, "%", p)
							} else if (R > 2) {
								S[A--] ? C += 4 : C += 2 * (T = H[C]) - 2;
							} else if (R > 0) {
								for (T = H[C], p = "", N = n.q[T][0]; N < n.q[T][1]; N++) {
									p += String.fromCharCode(o ^ n.p[N]);
									// console.log("webmssdk 2-103", String.fromCharCode(o ^ n.p[N]), "=", o, "^", n.p[N])
								}
								S[++A] = p, C += 4;
								// console.log("webmssdk 2-11", "S[A]=", p)
							}
						} else {
							R > 0 ? (R = E) < 1 ? S[++A] = h : R < 3 ? (
								tmp=S[A-1],
								p = S[A--], S[A] = S[A] + p
								// ,console.log("webmssdk 2-12", S[A], "=", tmp, "+", p)
								) : R < 5 && (p = S[A--],
							S[A] = S[A] == p) : (R = E) > 13 ? (S[++A] = H[C], C += 4
								// ,console.log("webmssdk 2-14", "S[A]=", S[A])
								) : R > 11 ? (p = S[A--], 
							tmp=S[A],
							S[A] = S[A] >> p
							// ,console.log("webmssdk 2-13", S[A], "=", tmp, ">>", p)
							) : R > 9 ? (T = H[C], C += 2, p = S[A--], f[T] = p) : R > 7 ? (T = H[C], 
							C += 4, y = A + 1, S[A -= T - 1] = T ? S.slice(A, y) : []) : R > 0 && (p = S[A--], 
							S[A] = S[A] > p);
						}
					} else if (R > 0) {
						R = 3 & E;
						if (E >>= 2, R < 1) {
							if ((R = E) > 9) {} else if (R > 7) {
								tmp = S[A-1];
								p = S[A--], S[A] = S[A] & p;
								// console.log("webmssdk 2-15", S[A], "=", tmp, "&", p);
							} else if (R > 5) {
								T = H[C], C += 2, S[A -= T] = 0 === T ? new S[A]() : c(S[A], t(S.slice(A + 1, A + T + 1)));
							} else if (R > 3) {
								T = H[C];
								try {
									if (i[r][2] = 1, 1 == (p = G(a, C + 4, T - 3, [], f, _, null, 0))[0]) {
										// console.log("webmssdk 5-5", "p=", p);
										return p;
									}
								} catch (b) {
									if (i[r] && i[r][1] && 1 == (p = G(a, i[r][1][0], i[r][1][1], [], f, _, b, 0))[0]) {
										// console.log("webmssdk 5-6", "p=", p);
										return p;
									}
								} finally {
									if (i[r] && i[r][0] && 1 == (p = G(a, i[r][0][0], i[r][0][1], [], f, _, null, 0))[0]) {
										// console.log("webmssdk 5-7", "p=", p);
										return p;
									}
									i[r] = 0, r--;
								}
								C += 2 * T - 2;
							}
						} else if (R < 2) {
							if ((R = E) > 12) {
								S[++A] = H[C], C += 2;
								// console.log("webmssdk 2-16", "S[A]=", S[A]);
							} else if (R > 10) {
								tmp = S[A-1];
								p = S[A--], S[A] = S[A] << p;
								// console.log("webmssdk 2-17", S[A], "=", tmp, "<<", p)
							} else if (R > 8) {
								for (T = H[C], R = "", N = n.q[T][0]; N < n.q[T][1]; N++) {
									R += String.fromCharCode(o ^ n.p[N]);
									// console.log("webmssdk 2-104", String.fromCharCode(o ^ n.p[N]), "=", o, "^", n.p[N])
								}
								tmp = S[A];
								C += 4, S[A] = S[A][R];
								// console.log("webmssdk 2-18", tmp, "=", S[A])
							} else {
								R > 6 && (y = S[A--], p = delete S[A--][y]);
							}
						} else {
							R < 3 ? (R = E) < 2 ? S[++A] = p : R < 11 ? (p = S[A -= 2][S[A + 1]] = S[A + 2], 
							A--) : R < 13 && (
								p = S[A], S[++A] = p
								// ,console.log("webmssdk 2-19", S[A], "=", p)
								) : (R = E) > 12 ? S[++A] = _ : R > 5 ? (p = S[A--], 
							S[A] = S[A] !== p) : R > 3 ? (
								tmp = S[A-1],
								p = S[A--], S[A] = S[A] / p
								// ,console.log("webmssdk 2-20", S[A], "=", tmp, "/", p)
							) : R > 1 ? C += 2 * (T = H[C]) - 2 : R > -1 && (S[A] = !S[A]);
						}
					} else {
						var P;
						R = 3 & E;
						if (E >>= 2, R < 1) {
							if ((R = E) > 14) {
								T = H[C], (P = function b() {
									var e = arguments;
									return b.y > 0 ? z(a, b.c, b.l, e, b.z, this, null, 0) : (b.y++, z(a, b.c, b.l, e, b.z, this, null, 0));
								}).c = C + 4, P.l = T - 2, P.x = G, P.y = 0, P.z = f, S[A] = P, C += 2 * T - 2;
							} else if (R > 12) {
								y = S[A--], m = S[A--], (R = S[A--]).x === G ? R.y >= 1 ? S[++A] = z(a, R.c, R.l, y, R.z, m, null, 1) : (S[++A] = z(a, R.c, R.l, y, R.z, m, null, 0), 
								// console.log("webmssdk 2-21", "S[A]=", S[A]),
								R.y++) : 
								(
									S[++A] = R.apply(m, y)
									, console.log("webmssdk 2-22", "S[A]=", S[A], "R=", R, "m=", m, "y=", y)
								);
							} else if (R > 5) {
								p = S[A--], S[A] = S[A] != p;
							} else if (R > 3) {
								tmp = S[A-1];
								p = S[A--], S[A] = S[A] * p;
								// console.log("webmssdk 2-23", S[A], "=", tmp, "*", p);
							} else if (R > -1) {
								// console.log("webmssdk 5-10", "S[A]=", S[A]);
								return [ 1, S[A--] ];
							}
						} else if (R < 2) {
							(R = E) < 4 ? (y = S[A--], (R = S[A]).x === G ? R.y >= 1 ? (S[A] = z(a, R.c, R.l, [ y ], R.z, m, null, 1)
							// ,console.log("webmssdk 2-24", "S[A]=", S[A])
							) : (S[A] = z(a, R.c, R.l, [ y ], R.z, m, null, 0), 
							// console.log("webmssdk 2-25", "S[A]=", S[A]),
							R.y++) : (S[A] = R(y)
							// ,console.log("webmssdk 2-26", "S[A]=", S[A], "R=", R, "y=", y)
							)) : R < 6 ? (
								tmp = S[A-1],
								S[A -= 1] = S[A][S[A + 1]]
								// ,console.log("webmssdk 2-105", tmp, "=", S[A])
								) : R < 10 && (p = S[A--], 
							S[A] = typeof p);
						} else if (R < 3) {
							if ((R = E) > 11) {
								throw S[A--];
							}
							if (R > 7) {
								for (p = S[A--], T = H[C], R = "", N = n.q[T][0]; N < n.q[T][1]; N++) {
									R += String.fromCharCode(o ^ n.p[N]);
									// console.log("webmssdk 2-106", String.fromCharCode(o ^ n.p[N]), "=", o, "^", n.p[N]);
								}
								C += 4, S[A--][R] = p;
								// console.log("webmssdk 2-107", "S[A]=", p);
							}
						} else {
							(R = E) < 1 ? S[++A] = null : R < 3 ? (p = S[A--], S[A] = S[A] >= p) : R < 12 && (S[++A] = void 0);
						}
					}
				}
			}
			return [ 0, null ];
		}
	```
	- 源码降低了代码可读性, 无法通过还原全部加密逻辑, 但代码表现出来的执行逻辑类似 switch-case 语句
		- ![webmssdk 第一个断点.gif](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/webmssdk%20%E7%AC%AC%E4%B8%80%E4%B8%AA%E6%96%AD%E7%82%B9.gif)
	- 将定位处的变量输出, 如上 `console.log("webmssdk 2-22", "S[A]=", S[A], "R=", R, "m=", m, "y=", y)`, 得到如下输出
		- ![解释 jsvmp 逻辑-02.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E8%A7%A3%E9%87%8A%20jsvmp%20%E9%80%BB%E8%BE%91-02.png)
		- 可以看到在加密流程的数据通过 `S[A]` 进行操作
		- 查看运行基本流程
			- ![解释 jsvmp 逻辑-03.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E8%A7%A3%E9%87%8A%20jsvmp%20%E9%80%BB%E8%BE%91-03.png)
		- 从源码可以看到, 存在多次操作 `S[A]` 的地方, 将所有可能与加密有关的地方添加断点, 输出后查看日志
			- 以下为打断点的技巧
				- 特别关注**位操作**和**四则运算**的部分, 需要操作前后数据以便于观察方式打印输出
					- ![位操作类的断点.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E4%BD%8D%E6%93%8D%E4%BD%9C%E7%B1%BB%E7%9A%84%E6%96%AD%E7%82%B9.png)
				- 特别关注**函数处理**的部分, 需将函数和函数实参打印输出
					- ![函数处理类的断点.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E5%87%BD%E6%95%B0%E5%A4%84%E7%90%86%E7%B1%BB%E7%9A%84%E6%96%AD%E7%82%B9.png)
				- 不关注输出数据类型为 boolean 结果
					- ![输出数据类型为 boolean 类的断点.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E8%BE%93%E5%87%BA%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B%E4%B8%BA%20boolean%20%E7%B1%BB%E7%9A%84%E6%96%AD%E7%82%B9.png)
		- 因此, 还原加密算法, 需要在可能加密的位置插入日志点, 输出运行时数据

- 由于使用异步和 VM 虚拟机, 无法直接在【作用域】查看, 采用打桩的方式, 使用 `console.log` 在可能加密的位置输出
	- ![webmssdk 第一个断点.gif](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/webmssdk%20%E7%AC%AC%E4%B8%80%E4%B8%AA%E6%96%AD%E7%82%B9.gif)
- 刷新页面，重新发送请求后, 打开【控制台】可以看到如下内容![第一个断点输出.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E7%AC%AC%E4%B8%80%E4%B8%AA%E6%96%AD%E7%82%B9%E8%BE%93%E5%87%BA.png)
- 可以将其保存在本地查看
	- ![浏览器日志保存至本地.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E6%B5%8F%E8%A7%88%E5%99%A8%E6%97%A5%E5%BF%97%E4%BF%9D%E5%AD%98%E8%87%B3%E6%9C%AC%E5%9C%B0.png)
- 在本地文件搜索 `X-Bogus` ![查找 X-Bogus.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E6%9F%A5%E6%89%BE%20X-Bogus.png)
- 搜索 `X-Bogus` 的值 `DFSzswVLbyhANxxXtm77gRXAIQ2X` , 定位第一次出现的位置
	- ![定位 X-Bogus 每一个生成逻辑.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E5%AE%9A%E4%BD%8D%20X-Bogus%20%E6%AF%8F%E4%B8%80%E4%B8%AA%E7%94%9F%E6%88%90%E9%80%BB%E8%BE%91.png)
	- ![查找源码中该行执行的语句.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E6%9F%A5%E6%89%BE%E6%BA%90%E7%A0%81%E4%B8%AD%E8%AF%A5%E8%A1%8C%E6%89%A7%E8%A1%8C%E7%9A%84%E8%AF%AD%E5%8F%A5.png)
		- 该语句执行操作 R(y), 并将函数 R 中 this 指针替换为 m
		- 由于控制台输出的 R 为函数普通文本, 日志中无法查看, 须在浏览器控制台查看
			- 亦可使用 `console.log(R.toString())`，但在函数字符较长时，会影响输出耗时
			- ![浏览器查看函数 R.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E6%B5%8F%E8%A7%88%E5%99%A8%E6%9F%A5%E7%9C%8B%E5%87%BD%E6%95%B0%20R.png)
		- R 为 `js` 内置函数 `chatAt`, `chars.charAt(i)`即 python 语言中 `chars[i]`
	- 通过以上观察，确认 X-Bogus 最后一层生成逻辑: 根据索引从某一个字符串中取值
- 由于日志点少, 输出信息不够完整, 修改本地代码
- 在新的日志中重新定位 `X-bogus` 第一次出现的位置
- 在之前的日志中, `X-Bogus` 似乎是每四个一组输出, 从倒数第 4 个开始查看
	- 倒数第四个
		- ![倒数第四个字符生成逻辑.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E5%80%92%E6%95%B0%E7%AC%AC%E5%9B%9B%E4%B8%AA%E5%AD%97%E7%AC%A6%E7%94%9F%E6%88%90%E9%80%BB%E8%BE%91.png)
	- 倒数第三个
		- ![倒数第三个字符生成逻辑.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E5%80%92%E6%95%B0%E7%AC%AC%E4%B8%89%E4%B8%AA%E5%AD%97%E7%AC%A6%E7%94%9F%E6%88%90%E9%80%BB%E8%BE%91.png)
	- 倒数第二个
		- ![倒数第二个字符生成逻辑.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E5%80%92%E6%95%B0%E7%AC%AC%E4%BA%8C%E4%B8%AA%E5%AD%97%E7%AC%A6%E7%94%9F%E6%88%90%E9%80%BB%E8%BE%91.png)
	- 倒数第一个
		- ![倒数第一个字符生成逻辑.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E5%80%92%E6%95%B0%E7%AC%AC%E4%B8%80%E4%B8%AA%E5%AD%97%E7%AC%A6%E7%94%9F%E6%88%90%E9%80%BB%E8%BE%91.png)
	- 画出以上数据运算流程
- 这四个只是一组, 为了观察规律, 继续向上观察 1-2 两组, 确定固定数据
- 由此, 可确定这一层的固定数据, 变化数据和运算逻辑
	- 固定数据: 65 位字符串(`Dkdpgh4ZKsQB80/Mfvw36XI1R25-WUAlEi7NLboqYTOPuzmFjJnryx9HVGcaStCe=`)
	- 变化数据: 21 位乱码
		- 显示数据: ![21 位乱码.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/21%20%E4%BD%8D%E4%B9%B1%E7%A0%81.png)
		- 对应的 python 字符串: `\x02ÿ-%.$Iô^\x8F]Uöè²N\x85^X¦q`
	- 加密逻辑（python 代码）
		```python
		    def get_char(stable_chars, grabled_chars):
	        ## 根据乱码和固定字符获取加密后字符
	        ## stable_chars="Dkdpgh4ZKsQB80/Mfvw36XI1R25-WUAlEi7NLboqYTOPuzmFjJnryx9HVGcaStCe="
		        RESULT_RIGHT_SHIFT = [18, 12, 6, 0]
		        res = ""
		        for index in range(len(grabled_chars)//3):
		            for _result_right_shift in RESULT_RIGHT_SHIFT:
		                grab_char_0 = grabled_chars[3*index+0]
		                grab_char_1 = grabled_chars[3*index+1]
		                grab_char_2 = grabled_chars[3*index+2]
		                res += stable_chars[
		                    ((((grab_char_0<<16)|(grab_char_1<<8))|grab_char_2)
		                        &(2**(_result_right_shift+6)-2**_result_right_shift))
                    >>		_result_right_shift
		                ]
		        return res
		```
		- 经过分析代码逻辑，以上为自定义字符的 [base64 算法](https://blog.csdn.net/duan196_118/article/details/122896400)
		- 
		
- 继续跟踪 21 位乱码或字符串第一次出现的位置
	- ![21 位乱码第一次出现的位置.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/21%20%E4%BD%8D%E4%B9%B1%E7%A0%81%E7%AC%AC%E4%B8%80%E6%AC%A1%E5%87%BA%E7%8E%B0%E7%9A%84%E4%BD%8D%E7%BD%AE.png)
	- ![21 位字符第一次出现的位置.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/21%20%E4%BD%8D%E5%AD%97%E7%AC%A6%E7%AC%AC%E4%B8%80%E6%AC%A1%E5%87%BA%E7%8E%B0%E7%9A%84%E4%BD%8D%E7%BD%AE.png)
- 在日志中查看不到 21 位乱码或字符串生成的逻辑, 需要去查看输出的代码
	- ![第一次输出 21 乱码的代码位置.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E7%AC%AC%E4%B8%80%E6%AC%A1%E8%BE%93%E5%87%BA%2021%20%E4%B9%B1%E7%A0%81%E7%9A%84%E4%BB%A3%E7%A0%81%E4%BD%8D%E7%BD%AE.png)
- 在代码中查找 `S[A][R]` 被赋值的语句
	- ![[S[A][R] 被复制的语句.png]]
- 由此可知, 数据流为 `S[A]->p->S[A][R]->S[A]`
- 将之前遗漏的日志点进行输出, 再次查看日志
- 通过之前的方法定位
	- ![定位乱码第一次出现的位置-02.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E5%AE%9A%E4%BD%8D%E4%B9%B1%E7%A0%81%E7%AC%AC%E4%B8%80%E6%AC%A1%E5%87%BA%E7%8E%B0%E7%9A%84%E4%BD%8D%E7%BD%AE-02.png)
	- ![python 输出乱码前两位.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/python%20%E8%BE%93%E5%87%BA%E4%B9%B1%E7%A0%81%E5%89%8D%E4%B8%A4%E4%BD%8D.png)
	- 由上可知, 21 位乱码由两部分连接得到
	- 通过多次断点分析, 可知前两位为固定值
- 接下来跟踪 19 位乱码
	- 此时, 出现与之前同样的问题, 无法定位, 再次更改日志点
	- 无法一次输出所有日志点, 是由于一次性全部输出会消耗过多时间, 最终导致刷新页面后接口访问超时
- 更改日志后, 重新定位
	- ![19 位乱码重新定位后的日志.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/19%20%E4%BD%8D%E4%B9%B1%E7%A0%81%E9%87%8D%E6%96%B0%E5%AE%9A%E4%BD%8D%E5%90%8E%E7%9A%84%E6%97%A5%E5%BF%97.png)
	- 这里函数没有输出在日志中, 同样是考虑到接口访问时间问题, 如果希望输出, 可以将函数输出更改为 `func.toString()`
	- 代码逻辑
		- ![生成 19 位乱码的代码逻辑.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E7%94%9F%E6%88%90%2019%20%E4%BD%8D%E4%B9%B1%E7%A0%81%E7%9A%84%E4%BB%A3%E7%A0%81%E9%80%BB%E8%BE%91.png)
		- ![定位生成 19 位乱码的函数.gif](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E5%AE%9A%E4%BD%8D%E7%94%9F%E6%88%90%2019%20%E4%BD%8D%E4%B9%B1%E7%A0%81%E7%9A%84%E5%87%BD%E6%95%B0.gif)
			- 函数源码
			```js
				function _0x251df2(a, b) {
					let e, d = [], c = 0, f = "";
					for (let a = 0; a < 256; a++) {
						d[a] = a;
					}
					for (let b = 0; b < 256; b++) {
						c = (c + d[b] + a.charCodeAt(b % a.length)) % 256,
						e = d[b],
						d[b] = d[c],
						d[c] = e;
					}
					let t = 0;
					c = 0;
					for (let a = 0; a < b.length; a++) {
						t = (t + 1) % 256,
						c = (c + d[t]) % 256,
						e = d[t],
						d[t] = d[c],
						d[c] = e,
						f += String.fromCharCode(b.charCodeAt(a) ^ d[(d[t] + d[c]) % 256]);
					}
					return f;
				}
			```
	- 由上可知, 19 位乱码(garb_19_1)由函数 `_0x251df2(255, garb_19_2)` 生成
	- 可确定这一层的固定数据, 变化数据和加密逻辑
		- 固定数据: 255
		- 变化数据: 另一组 19 位乱码(garb_19_2)
			- <code>@\x00\x01\x009§E?d\x1Ee]`\x90XÂ°\x1A'</code>
		- 加密逻辑: 函数 `_0x251df2`
- 接下来跟踪 garb_19_2, 定位方法与之前一样
	- ![定位生成 garb_19_2 的输入和逻辑.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E5%AE%9A%E4%BD%8D%E7%94%9F%E6%88%90%20garb_19_2%20%E7%9A%84%E8%BE%93%E5%85%A5%E5%92%8C%E9%80%BB%E8%BE%91.png)
	- 使用 python 将以上数组进行转换，可以确定 garb_19_2 由以上数组生成
		- ![使用 python 计算 garb_19_2.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E4%BD%BF%E7%94%A8%20python%20%E8%AE%A1%E7%AE%97%20garb_19_2.png)
	- 由上可知, garb_19_2 生成的变化数据, 生成逻辑
		- 变化数据: `[64, 0.00390625, 1, 0, 57, 167, 69, 63, 100, 30, 101, 93, 96, 144, 88, 194, 176, 26, 39]` 
		- 生成逻辑: `chr()`, `int()`
		- 其中 `0.00390625` 需要转整数
- 接下来跟踪 19 位数组(array_19)
	- ![array_19 第一次出现的位置.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/array_19%20%E7%AC%AC%E4%B8%80%E6%AC%A1%E5%87%BA%E7%8E%B0%E7%9A%84%E4%BD%8D%E7%BD%AE.png)
	- 观察发现, 上述截图存在 `39=61^26`, 而 `26` 是 array_19 的倒数第二位
	- 继续向上观察
		- ![生成 19 为数组最后一位的逻辑.gif](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E7%94%9F%E6%88%90%2019%20%E4%B8%BA%E6%95%B0%E7%BB%84%E6%9C%80%E5%90%8E%E4%B8%80%E4%BD%8D%E7%9A%84%E9%80%BB%E8%BE%91.gif)
		- 由上推断, array_19 最后一位由前几位异或得到
		- 复制数据到 python 终端, 进行验证
			- ![在 python 终端验证最后一位运算逻辑.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E5%9C%A8%20python%20%E7%BB%88%E7%AB%AF%E9%AA%8C%E8%AF%81%E6%9C%80%E5%90%8E%E4%B8%80%E4%BD%8D%E8%BF%90%E7%AE%97%E9%80%BB%E8%BE%91.png)
	- 跟踪 array_19 的前 18 位
		- 第 15-18 位 
			- ![array_19 15-18 位运算逻辑.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/array_19%2015-18%20%E4%BD%8D%E8%BF%90%E7%AE%97%E9%80%BB%E8%BE%91.png)
			- 经多次日志确认, `1489154074` 为固定值
				- 当间隔较长时间重新测试时，该数据发生了变化，尚未确认该值的运算逻辑
		- 第 11-14 位
			- ![array_19 11-14 位计算逻辑.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/array_19%2011-14%20%E4%BD%8D%E8%AE%A1%E7%AE%97%E9%80%BB%E8%BE%91.png)
			- 经测试, `1700618384.293` 为秒级时间戳。在 python 运行时转换为整数即可
		- 第 9-10 位
			- ![array_19 12-13 位运算逻辑.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/array_19%2012-13%20%E4%BD%8D%E8%BF%90%E7%AE%97%E9%80%BB%E8%BE%91.png)
			- 在 python 终端测试可知, `0x64=100, 0x1e=30`
			- 定位字符 `01b95a66aa036f19e522ceec90f4641e`
				- ![定位 01b95a66aa036f19e522ceec90f4641e.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E5%AE%9A%E4%BD%8D%2001b95a66aa036f19e522ceec90f4641e.png)
				- `01b95a66aa036f19e522ceec90f4641e` 位固定长度, 猜测为 md5 算法
					- 使用 python 终端测试
						- ![定位 32 位字符生成逻辑-01.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E5%AE%9A%E4%BD%8D%2032%20%E4%BD%8D%E5%AD%97%E7%AC%A6%E7%94%9F%E6%88%90%E9%80%BB%E8%BE%91-01.png)
			- 定位 `eXNvqMlI8HJgDjFm4ywgQMRZ7wNTaM/6H+o8cziepd4Cga/SpGZB4ISfVF4cGizt7vLE3Hpm0W2wfnQDlRSKZmEa3j2hJtoZEQ1cJr2YghkKKyZwJy45LttMfZ+UugL09RAYK5HizsBzEgtS1/8i`
				- ![定位 148 位长字符.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E5%AE%9A%E4%BD%8D%20148%20%E4%BD%8D%E9%95%BF%E5%AD%97%E7%AC%A6.png)
				- 这 148 位长字符运算逻辑与之前 `X-Bogus` 最后一层逻辑一致
				- 148 长字符固定数据, 变化数据和运算逻辑
					- 固定数据: 65 位长字符(` ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/=`)
					- 变化数据: 111 位乱码
						- ![111 位乱码.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/111%20%E4%BD%8D%E4%B9%B1%E7%A0%81.png)
				- 定位 111 位乱码(garb_111_3)
					- ![定位 garb_111_3 初始位置.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E5%AE%9A%E4%BD%8D%20garb_111_3%20%E5%88%9D%E5%A7%8B%E4%BD%8D%E7%BD%AE.png)
					- ![定位生成浏览器 111 位乱码生成逻辑.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E5%AE%9A%E4%BD%8D%E7%94%9F%E6%88%90%E6%B5%8F%E8%A7%88%E5%99%A8%20111%20%E4%BD%8D%E4%B9%B1%E7%A0%81%E7%94%9F%E6%88%90%E9%80%BB%E8%BE%91.png)
					- 观察如上数据可确定计算 garb_111_3 的固定数据, 变化数据和运算逻辑
						- 固定数据: `[0, 1, 0]`
						- 变化数据: `Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/119.0.0.0 Safari/537.36`
							- 为请求头中的 `User-Agent`, 即 `headers[User-Agent]`
						- 运算逻辑: 函数 `_0x251df2`
		- 第 7-8 位
			- ![array_19 7-8 位生成逻辑.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/array_19%207-8%20%E4%BD%8D%E7%94%9F%E6%88%90%E9%80%BB%E8%BE%91.png)
				- 经多次测试, `d41d8cd98f00b204e9800998ecf8427e` 为固定值
		- 第 5-6 位
			- ![定位 array_19 5-6 位运算逻辑.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E5%AE%9A%E4%BD%8D%20array_19%205-6%20%E4%BD%8D%E8%BF%90%E7%AE%97%E9%80%BB%E8%BE%91.png)
			- 使用 python 终端测试运算逻辑
				- ![python 终端测试 md5 hash 逻辑.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/python%20%E7%BB%88%E7%AB%AF%E6%B5%8B%E8%AF%95%20md5%20hash%20%E9%80%BB%E8%BE%91.png)
			- 由上可知，第 5-6 位的运算逻辑，会将 hash 的中间字节串(bytes)迭代 hash
		- 第 1-4 位
			- 经测试，前 4 位亦为固定值, 使用 python 实现算法时, 固定为 `[64, 0, 1, 0]`
	- array_19 运算流程
- 以上, `X-Bogus` 生成逻辑已经还原, 画出 `X-Bogus` 算法流程图
- `X-Bogus` [源码](gen_x_bogus.py)
	```python
		## -*- coding: utf-8 -*-
		## @author <yy>
		## @date 2023-11-24
		## @detail 抖音网页的 X-Bogus(28 位) 生成算法
		
		import ctypes
		import hashlib
		import struct
		import urllib.parse
		
		from datetime import datetime
		
		class XBogus:
			## 更新密码时生成校验签名
			RESULT_RIGHT_SHIFT = [18, 12, 6, 0]
			STABLE_CHARS = {
				"s0": 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/=', 
				"s1": 'Dkdpgh4ZKsQB80/Mfvw36XI1R25+WUAlEi7NLboqYTOPuzmFjJnryx9HVGcaStCe=', 
				"s2": 'Dkdpgh4ZKsQB80/Mfvw36XI1R25-WUAlEi7NLboqYTOPuzmFjJnryx9HVGcaStCe=', 
				"s3": 'ckdp1h4ZKsUB80/Mfvw36XIgR25+WQAlEi7NLboqYTOPuzmFjJnryx9HVGDaStCe', 
				"s4": 'Dkdpgh2ZmsQB80/MfvV36XI1R45-WUAlEixNLwoqYTOPuzKFjJnry79HbGcaStCe'
			}
		
			STABLE_HASH_32 = "d41d8cd98f00b204e9800998ecf8427e"
		
			TRANSFORM_START_NUMBER = 0xff
		
			@classmethod
			def unsigned_right_shift(cls, number, bits=0):
				## 无符号右移
				number, bits = ctypes.c_uint32(number).value, bits % 32
				return ctypes.c_uint32(number >> bits).value
			
			def __init__(self, params: dict, user_agent:str, timestamp):
				self._params = params
				self._user_agent = user_agent
				self.timestamp = timestamp
		
			# @property
			# def timestamp(self):
			#     ## 以整数表示的当前时间的秒级时间戳
			#     return 1702093423
			#     # return 1702006425
			#     # return int(datetime.now().timestamp())
		
			def transform_chars_to_array(self, chars):
				## 将字符集转换为数组
				return [
					ord(_char)
					for _char in chars
				]
		
			@property
			def params(self):
				return urllib.parse.urlencode(self._params)
		
			@property
			def user_agent(self):
				return self.transform_chars_to_array(self._user_agent)
		
			def gen_test_pos(self, chars):
				## 生成校验位
				test_pos = chars[0]
				for char in chars[1:]:
					test_pos ^= char
				return test_pos
		
			@property
			def big_num_1(self):
				return 1489154074
		
			@classmethod
			def transform_hex_chars_to_byte(cls, chars):
				## chars 长度 2 的整数倍, 每两位代表一个 16 进制数
				length = len(chars)
				res_array = []
				for i in range(length//2):
					res_array.append(int(chars[i*2:(i+1)*2], 16))
				return bytes(res_array)
		
			@property
			def param_hash_tuple(self):
				return struct.unpack("16B", hashlib.md5(hashlib.md5(self.params.encode()).digest()).digest())
		
			@property
			def stable_array_hash_tuple(self):
				return struct.unpack("16B", hashlib.md5(self.transform_hex_chars_to_byte(self.STABLE_HASH_32)).digest())
		
			@property
			def transform_user_agent_array_256(self):
				## 对 111 位 User-Agent 进行转换的 256 位数组
				array_1 = list(range(256))
				array_len_3 = [0, 1, 0]
				tmp = 0
				for i in range(256):
					tmp = ((array_1[i]+tmp)+array_len_3[i%3])%256
					array_1[i], array_1[tmp] = array_1[tmp], array_1[i]
				return array_1
		
			@property
			def user_agent_grabled_chars(self):
				user_agent = self.user_agent
				transform_user_agent_array_256 = self.transform_user_agent_array_256
				tmp = 0
				for i in range(1, len(user_agent)+1):
					tmp = (tmp+transform_user_agent_array_256[i])%256
					tmp_index = (transform_user_agent_array_256[i]+transform_user_agent_array_256[tmp])%256
					transform_user_agent_array_256[i], transform_user_agent_array_256[tmp] = transform_user_agent_array_256[tmp], transform_user_agent_array_256[i]
					user_agent[i-1] = user_agent[i-1]^transform_user_agent_array_256[tmp_index]
				return user_agent
		
			@property
			def user_agent_screct_key(self):
				return self.get_char(
					self.STABLE_CHARS["s0"],
					self.user_agent_grabled_chars
				)
		
			@property
			def user_agent_hash_tuple(self):
				return struct.unpack("16B", hashlib.md5(self.user_agent_screct_key.encode()).digest())
		
			@property
			def grabled_chars_19_org(self):
				## 获取时间戳
				timestamp = self.timestamp
				big_num_1 = self.big_num_1
				
				res = [0]*19
				## 固定值
				res[0:4] = [64, 0, 1, 0]
		
				res[4:6] = self.param_hash_tuple[-2:]
				res[6:8] = self.stable_array_hash_tuple[-2:]
				res[8:10] = self.user_agent_hash_tuple[-2:]
		
				res[10] = (timestamp>>24)&255
				res[11] = (timestamp>>16)&255
				res[12] = (timestamp>>8)&255
				res[13] = (timestamp>>0)&255
		
				res[14] = (big_num_1>>24)&255
				res[15] = (big_num_1>>16)&255
				res[16] = (big_num_1>>8)&255
				res[17] = (big_num_1>>0)&255
		
		
				res[18] = self.gen_test_pos(res[:-1])
		
				return res
		
			@property
			def transform_char_19_array_256(self):
				## 二次转换乱码字符串的 256 位数组
		
				array_1 = list(range(256))
				tmp = 0
				for i in range(256):
					tmp = (tmp+array_1[i]+self.TRANSFORM_START_NUMBER)%256
					array_1[i], array_1[tmp] = array_1[tmp], array_1[i]
				return array_1
		
			@property
			def grabled_chars_19(self):
				grabled_chars_19_org = self.grabled_chars_19_org
				transform_char_19_array_256 = self.transform_char_19_array_256
				tmp = 0
				for i in range(1, 20):
					tmp = (transform_char_19_array_256[i]+tmp)%256
					tmp_index = (transform_char_19_array_256[i]+transform_char_19_array_256[tmp])%256
					## 将数据进行交换，保证获取的一致性
					transform_char_19_array_256[i], transform_char_19_array_256[tmp] = transform_char_19_array_256[tmp], transform_char_19_array_256[i]
					grabled_chars_19_org[i-1] = grabled_chars_19_org[i-1]^transform_char_19_array_256[tmp_index]
				return grabled_chars_19_org
		
			@property
			def grabled_chars_2(self):
				return [
					2,
					255
				]
		
			@property
			def grabled_chars(self):
				return self.grabled_chars_2+self.grabled_chars_19
		
			def get_char(self, stable_chars, grabled_chars):
				## 根据乱码和固定字符获取加密后字符
				res = ""
				for index in range(len(grabled_chars)//3):
					for _result_right_shift in self.RESULT_RIGHT_SHIFT:
						grab_char_0 = grabled_chars[3*index+0]
						grab_char_1 = grabled_chars[3*index+1]
						grab_char_2 = grabled_chars[3*index+2]
						res += stable_chars[
							((((grab_char_0<<16)|(grab_char_1<<8))|grab_char_2)
								&(2**(_result_right_shift+6)-2**_result_right_shift))\
								>>_result_right_shift
							]
					return res
			
				@property
				def x_bogus(self):
					grabled_chars = self.grabled_chars
					return self.get_char(
						self.STABLE_CHARS["s2"],
						grabled_chars
					)
			
			def gen_x_bogus(params, user_agent, timestamp):
				return XBogus(params, user_agent, timestamp).x_bogus
			
			
			if __name__ == "__main__":
				params = {
					# 'X-Bogus': 'DFSzswVLEWkANxxXtmnw6RXAIQ59',
					'password': 'qxkj20130406-gf2',
					'employee_sec_uid': 'MS4wLjABAAAAEu5iHZ9ytYQ3bGEXtj6oCJLJB0-KohRnSX0bKjTao3w',
					'msToken': '',
					# '_signature': '_02B4Z6wo00001cjv3KwAAIDAq-Ucx6hFiPXI79gAABdlO9q1Yc-.ONBRMmMr0BOwhPf9m1jxqV08SZVP6U6b721o0Ks66jOWrR4EfjXka5cEWzZt58TZVWExKmlBEj55Yi5HMohVDuU2v26ac8',
				}
				print("X-Bogus", gen_x_bogus(
					params,
					'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/119.0.0.0 Safari/537.36',
					1702093423,
				))
		```

## 逆向参数 `_signature`
- 定位 `_signature` 第一次出现的位置
	- 由于 `_signature` 长度为 147, 未使用 json 序列化打印时, 长度会被压缩为 100 以内, 无法通过 `_signature` 搜索
		- ![定位 _signature.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E5%AE%9A%E4%BD%8D%20_signature.png)
	- 通过观察接口中请求, 在日志中搜索。同时注意, 只用前几位搜索即可, 否则, 可能无法定位
		- ![接口请求中的 _signature.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E6%8E%A5%E5%8F%A3%E8%AF%B7%E6%B1%82%E4%B8%AD%E7%9A%84%20_signature.png)
	- `_signature` 基本结构
		- ![_signature 基本结构.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/_signature%20%E5%9F%BA%E6%9C%AC%E7%BB%93%E6%9E%84.png)
	- 通过上图观察, 可以确定 `_signature` 结构
		- 14 位(`_signature[0:14]`): `_02B4Z6wo00001`
			- 经多次测试, 为固定值
		- 31 位(`_signature[14:45]`)
		- 100 位(`_signature[45:145]`)
		- 2 位(`_signature[145:147]`)
		- 接下来，需要不断在日志中定位对应位置第一次出现的位置，直至能够逆推生成逻辑
			- 其中，若是在第一次出现位置之前无法继续查看，则需要检查源码，应该是缺失了日志点，未能正确输出
	- 其中，最后两位为校验位, 其计算的固定数值, 变化数值和运算逻辑定位如下
		- ![_signature 校验位生成逻辑.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/_signature%20%E6%A0%A1%E9%AA%8C%E4%BD%8D%E7%94%9F%E6%88%90%E9%80%BB%E8%BE%91.png)
		- ![定位生成 _signature 校验位的函数.gif](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E5%AE%9A%E4%BD%8D%E7%94%9F%E6%88%90%20_signature%20%E6%A0%A1%E9%AA%8C%E4%BD%8D%E7%9A%84%E5%87%BD%E6%95%B0.gif)
		- 固定数值: 0
		- 变化数值: `_signature[:145]`
		- 运算逻辑
	
			```js
				function _0x2521e4(a, b) {
					// a=0, b=_signature[:145]
					for (let e = 0; e < b.length; e++) {
						let d = b.charCodeAt(e);
						if (d >= 55296 && d <= 56319 && e < b.length) {
							const a = b.charCodeAt(e + 1);
							56320 == (64512 & a) && (d = ((1023 & d) << 10) + (1023 & a) + 65536,
							e += 1);
						}
						a = 65599 * a + d >>> 0;
					}
					return a;
				}
			```
	- 其中，`_signature[14:45]` 生成逻辑如下
		- ![_signature 31 位生成逻辑.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/_signature%2031%20%E4%BD%8D%E7%94%9F%E6%88%90%E9%80%BB%E8%BE%91.png)
		- ![复原 31 位字符运算基本逻辑.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E5%A4%8D%E5%8E%9F%2031%20%E4%BD%8D%E5%AD%97%E7%AC%A6%E8%BF%90%E7%AE%97%E5%9F%BA%E6%9C%AC%E9%80%BB%E8%BE%91.png)
		- 观察以上图片可知，`_signature[14:45]` 基本每 5 个字符为 1 组生成
		- 其中转换字符的最后一步类似 base64, 会将数据转换为 64 以内数据进行如下映射
			- ![字符映射表.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E5%AD%97%E7%AC%A6%E6%98%A0%E5%B0%84%E8%A1%A8.png)

			- 根据逻辑进行还原，则可转换为取字符串 `char_64_1` 对应位置字符
				- `char_64_1 = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789-."`
		- 其中生成 5 个字符的固定数值，变化数值和运算逻辑如下
			- 固定数值: 24, 18, 12, 6, 0, 63, 65, 71, -4, -17
			- 变化数字: `number`
			- 运算逻辑
				```python
					def get_signature_5_chars(number, stable_chars):
						_chars = ""
						for bits in [24, 18, 12, 6, 0]:
							_chars += stable_chars[(number>>bits)&63]
						return _chars
				```
		- 同时发现，最终字符串长度为 31，不是 5 的整数倍
		- 继续观察发现，第 3 组字符串长度长度为 6，其中前 5 生成逻辑与其他字符串无异，最后一位由中间数据生成
			- ![第三组字符串生成逻辑.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E7%AC%AC%E4%B8%89%E7%BB%84%E5%AD%97%E7%AC%A6%E4%B8%B2%E7%94%9F%E6%88%90%E9%80%BB%E8%BE%91.png)
		- 由此可知，只需要确定生成 5 位字符的 `number` 即可
			- 其中，需要定位一个超大数 `super_big_number`
				- ![超大数出现的位置.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E8%B6%85%E5%A4%A7%E6%95%B0%E5%87%BA%E7%8E%B0%E7%9A%84%E4%BD%8D%E7%BD%AE.png)
			- ![超大数运算逻辑.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E8%B6%85%E5%A4%A7%E6%95%B0%E8%BF%90%E7%AE%97%E9%80%BB%E8%BE%91.png)
			- 定位运算函数
				- ![定位计算超大数中数字和字符运算逻辑.gif](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E5%AE%9A%E4%BD%8D%E8%AE%A1%E7%AE%97%E8%B6%85%E5%A4%A7%E6%95%B0%E4%B8%AD%E6%95%B0%E5%AD%97%E5%92%8C%E5%AD%97%E7%AC%A6%E8%BF%90%E7%AE%97%E9%80%BB%E8%BE%91.gif)
				- 多次测试发现，函数 `_0x435fc6` 为数字与字符串进行运算的基础函数
			- 由上可得，计算超大数的固定数值, 变化数值和运算逻辑
				- 固定数值: 0, 10000000110000, 65521
				- 变化数值: 时间戳, `headers["referer"]`
				- 运算逻辑: 【计算流程图】

		- 每一个 `number`  运算逻辑如下
			- `number1`: `number1 = super_big_number>>2`
				- 如用 python  实现，需要注意位操作。因为 python 的整数具有无限精度，即使 `super_big_number` 长度超过 32 位，也不会出现溢出；但在 js 中却会在运算前将数据转换为有符号 32 位
				- 有以下两种解决方案
					- python 原生: 使用 `ctypes` 将计算前数据强制转换为有符号 32 位
						```python
							import ctypes
							## 输出: -25000298
							ctypes.c_int32(35394725485145).value >> 2
						```
					- 第三方包: 使用 python 运算 js 简短语句。使用第三方包 `js2py`
						- 安装第三方包: `pip install js2py`
						```python
							import js2py
							super_big_number = 35394725485145
							## 输出: -25000298
							js2py.eval_js(f"{super_big_number}>>2")
						```
			- `number2`
				- ![number2 计算逻辑.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/number2%20%E8%AE%A1%E7%AE%97%E9%80%BB%E8%BE%91.png)
				- 多次测试发现, `4294967296` 为固定值, 定义为 `stable_number_1`
				- 记 `tmp1=(super_big_number/stable_number_1)>>>0`
				- 由上图可得，`number2 = (super_big_number<<28)|(tmp1>>>4)`
				- 算法流程图如下
					- 【number2 算法流程图】
			- `number3`
				- ![number3 计算逻辑.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/number3%20%E8%AE%A1%E7%AE%97%E9%80%BB%E8%BE%91.png)
				- 多次测试发现，`1489154074` 为固定值，定义为 `stable_number_2`
				- 记 `tmp2=stable_number_2^super_big_number`
				- 由上图可得，`number3 = (tmp1<<26)|(tmp2>>>6)`
				- 算法流程图如下
					- 【number3 算法流程图】
				- 其中 `tmp2` 在计算完第三组的 5 个字符之后，用于计算第 6 个字符
			- `number4`
				- ![number4 计算逻辑-01.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/number4%20%E8%AE%A1%E7%AE%97%E9%80%BB%E8%BE%91-01.png)
				- ![number4 计算逻辑-02.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/number4%20%E8%AE%A1%E7%AE%97%E9%80%BB%E8%BE%91-02.png)
				- 在浏览器控制台查看，以上由数字和字符串进行计算的函数为 `_0x435fc6`
				- 定义 `tmp3 = _0x435fc6(0, super_big_number.toString())`
				- 定义 `user_agent = "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/119.0.0.0 Safari/537.36"`
				- 定义 `params` 为 URL 请求参数经 `urllib.parse.urlencode` 格式化而得到的字符串
				- 定义 `tmp5 = _0x435fc6(tmp3, user_agent)`
				- 定义 `tmp6 = _0x435fc6(tmp3, params)`
				- 定义 `tmp7 = ((tmp5%65521)<<16)|(tmp6%65521)`
				- 由上可得，`number4 = tmp7>>2`
				- 算法流程图如下
					- 【number4 算法流程图】
			- `number5`
				- ![number5 计算逻辑-01.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/number5%20%E8%AE%A1%E7%AE%97%E9%80%BB%E8%BE%91-01.png)
				- ![number5 计算逻辑-02.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/number5%20%E8%AE%A1%E7%AE%97%E9%80%BB%E8%BE%91-02.png)
				- 由上可得，`number6 = (tmp7>>28)|((((1<<8)|(2<<4))^super_big_number)>>>4)`
				- 算法流程图如下
					- 【number5 算法流程图】
			- `number6`
				- ![number6 计算逻辑.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/number6%20%E8%AE%A1%E7%AE%97%E9%80%BB%E8%BE%91.png)
				- 明显发现, `number6` 是计算 `super_big_number` 的中间变量
		- 由上可画出 `_signature[14:45]` 的运算算法流程图
			- ![_signautre_31 计算算法流程图.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/_signautre_31%20%E8%AE%A1%E7%AE%97%E7%AE%97%E6%B3%95%E6%B5%81%E7%A8%8B%E5%9B%BE.png)
	- 其中，`_signature[45:145]` 生成逻辑如下
		- 通过多次测试发现，`_signature[45:145]` 在计算 `X-Bogus` 之前已经生成。其生成的时间是刷新页面之后。
		- 在刷新页面之后，查看其运算逻辑
			- ![_byted_sec_did 运算逻辑.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/_byted_sec_did%20%E8%BF%90%E7%AE%97%E9%80%BB%E8%BE%91.png)
		- 由上可知，`_signature[45:100] = chars_4+chars_96`
		- `chars_4` 计算逻辑
			- ![随机 4 位字符串计算逻辑.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E9%9A%8F%E6%9C%BA%204%20%E4%BD%8D%E5%AD%97%E7%AC%A6%E4%B8%B2%E8%AE%A1%E7%AE%97%E9%80%BB%E8%BE%91.png)
			- ![获取随机 4 位字符串的运算逻辑.gif](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E8%8E%B7%E5%8F%96%E9%9A%8F%E6%9C%BA%204%20%E4%BD%8D%E5%AD%97%E7%AC%A6%E4%B8%B2%E7%9A%84%E8%BF%90%E7%AE%97%E9%80%BB%E8%BE%91.gif)
				```js
					function _0xceda58(a, b) {
						b || (b = "0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ");
						let e = "";
						for (let d = a; d > 0; --d) {
							e += b[Math.floor(Math.random() * b.length)];
						}
						// console.log("webmssdk 16-1", "a=", a, "b=", b, "e=", e)
						return e;
					}
				```
			- 由上可知，`chars_4` 为随机生成的 4 为字符串，最后会拼接到 `_signature[45:100]` 的开头
		- `chars_96` 计算逻辑
			- ![96 位字符计算逻辑.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/96%20%E4%BD%8D%E5%AD%97%E7%AC%A6%E8%AE%A1%E7%AE%97%E9%80%BB%E8%BE%91.png)
			- ![96 位字符运算逻辑函数.gif](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/96%20%E4%BD%8D%E5%AD%97%E7%AC%A6%E8%BF%90%E7%AE%97%E9%80%BB%E8%BE%91%E5%87%BD%E6%95%B0.gif)
			- 由上发现，96 位字符由 72 位字符生成而来，类似 base64
			- 由此，只需要确定 base64 输入的 72 位乱码即可
				- ![72 位乱码计算逻辑.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/72%20%E4%BD%8D%E4%B9%B1%E7%A0%81%E8%AE%A1%E7%AE%97%E9%80%BB%E8%BE%91.png)
				- ![72 位乱码计算函数.gif](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/72%20%E4%BD%8D%E4%B9%B1%E7%A0%81%E8%AE%A1%E7%AE%97%E5%87%BD%E6%95%B0.gif)
				- 以下为计算过程所需执行的全部代码	
					```js
						function _0xceda58(a, b) {
							b || (b = "0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ");
							let e = "";
							for (let d = a; d > 0; --d) {
								e += b[Math.floor(Math.random() * b.length)];
							}
							// a= 4 b= 0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ e= CXO9
							// return e;
							return "k4kF"
						}
						function _0x2beca2(a, b) {
							// a = 68 位字符, b = true
							// b = 4 位字符, b = false
							var e, d = a.length, c = d >> 2;
							0 != (3 & d) && ++c, b ? (e = new Array(c + 1))[c] = d : e = new Array(c);
							for (let b = 0; b < d; ++b) {
								e[b >> 2] |= a.charCodeAt(b) << ((3 & b) << 3);
							}
							return e;
						}
						function _0x494edc(a) {
							return 4294967295 & a;
						}
						function _0x58ebdf(a, b, e, d, c, f) {
							return (e >>> 5 ^ b << 2) + (b >>> 3 ^ e << 4) ^ (a ^ b) + (f[3 & d ^ c] ^ e);
						}
						function _0x2d5686(a, b) {
							// 一堆大数进行计算
							// a = [17+1], b = 1(length=4)
							var e, d, c, f, t, n, o = a.length, i = o - 1;
							for (d = a[i], c = 0, n = 0 | Math.floor(6 + 52 / o); n > 0; --n) {
								for (f = (c = _0x494edc(c + 2654435769)) >>> 2 & 3, t = 0; t < i; ++t) {
									e = a[t + 1], d = a[t] = _0x494edc(a[t] + _0x58ebdf(c, e, d, t, f, b));
								}
								e = a[0], d = a[i] = _0x494edc(a[i] + _0x58ebdf(c, e, d, i, f, b));
								console.log(n, a)
							}
							return a;
						}
						function _0x4d469b(a, b) {
							var e = a.length, d = e << 2;
							if (b) {
								var c = a[e - 1];
								if (c < (d -= 4) - 3 || c > d) {
									return null;
								}
								d = c;
							}
							for (var f = 0; f < e; f++) {
								a[f] = String.fromCharCode(255 & a[f], a[f] >>> 8 & 255, a[f] >>> 16 & 255, a[f] >>> 24 & 255);
							}
							var t = a.join("");
							return b ? t.substring(0, d) : t;
						}
						function _0x4eb0f0(a) {
							return a.length < 4 && (a.length = 4), a;
						}
						// 主函数，输入为 a=68 为字符串，b=4 位随机字符串
						function _0x311da1(a, b) {
							// a=_raw_sec_did, b=随机 4 为字符串
							return null == a || 0 === a.length ? a : (a = _0x279d57(a),
							b = _0x279d57(b),
							_0x4d469b(_0x2d5686(_0x2beca2(a, !0), _0x4eb0f0(_0x2beca2(b, !1))), !1));
						}
					```
				- 上面的函数 尽管运算过程复杂，但主函数的输入输出却很明确，可以直接使用 python 调用 js 代码，规避还原其中的复杂逻辑
					- 也可以一步步阅读源码将 js 转 python
				- 由此，只需要确定输入的 68 位字符即可
					- 在 G 函数位置，手动添加断点
						- ![定位 _raw_sec_did 手动添加断点.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E5%AE%9A%E4%BD%8D%20_raw_sec_did%20%E6%89%8B%E5%8A%A8%E6%B7%BB%E5%8A%A0%E6%96%AD%E7%82%B9.png)
					- ![进入手动断点.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E8%BF%9B%E5%85%A5%E6%89%8B%E5%8A%A8%E6%96%AD%E7%82%B9.png)
					- ![定位 68 位字符到 cookie.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E5%AE%9A%E4%BD%8D%2068%20%E4%BD%8D%E5%AD%97%E7%AC%A6%E5%88%B0%20cookie.png)
						- 获取 68 位字符的函数
							```js
								function _0x56bb8b(a) {
									try {
										let b = "";
										return window.sessionStorage && (b = window.sessionStorage.getItem(a),
										b) ? b : window.localStorage && (b = window.localStorage.getItem(a),
										b) ? b : (b = _0x1478aa(a, document.cookie),
										b);
									} catch (a) {
										return "";
									}
								}
							```
						- 观察以上函数不难发现，程序一次从 session 存储、本地存储和 cookie 中获取 tt_scid 的值作为 68 位字符
						- 以 `tt_scid` 为关键字进行检索，确定 tt_scid 生成位置
							- ![定位 tt_scid 生成位置.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E5%AE%9A%E4%BD%8D%20tt_scid%20%E7%94%9F%E6%88%90%E4%BD%8D%E7%BD%AE.png)
						- 在 tt_scid 写入位置，添加语句，将 tt_scid 锁定为固定值，便于调试
							- 在 tt_scid 写入之前，注入语句 `a.fp="iaSFzmAk56ZDG-HbkbVu7OKvPiuJUwsrZqbn1kp5uIu6xg8W0ec6aQj2-VzlsI0M2895";` 
							- ![注入 tt_scid.png](/img/user/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/01-%E9%80%86%E5%90%91%E6%8A%96%E9%9F%B3%E4%BC%81%E4%B8%9A%E5%8F%B7%20X-Bogus%20%E5%92%8C%20_signature%20%E5%8F%82%E6%95%B0/%E6%B3%A8%E5%85%A5%20tt_scid.png)
						- 经测试 tt_scid 未进行检验，使用固定值，不再还原其生成逻辑
	- 至此，`_signature` 生成逻辑已完全还原，可画出 `_signature` 生成算法流程图

## 排查问题
- 使用模拟数据，能够生成完全一致的参数 `X-Bogus` 和 `_signature`
- 但当取消模拟数据之后，接口仍然返回 403
- 使用抓包工具 fiddler 调试发现，即使强制将 `X-Bogus` 和 `_signature` 赋值为空字符，接口依然访问成功
- 因此，断定接口并未校验参数 `X-Bogus` 和 `_signature`
- 通过使用观察及调试，发现导致参数返回 403 的参数为 cookie 中 `csrf_session_id` 和 `x-secsdk-csrf-token`

## 逆向 `csrf_session_id` 和 `x-secsdk-csrf-token` 

## 参考资料
- [X-Bogus 还原](https://cloud.tencent.com/developer/article/2208864)
- [a_bogus 还原](https://blog.csdn.net/baidu_39403331/article/details/133012562)
- [掘金 a_bogus](https://juejin.cn/post/7276257296497885236)
- [使用 github 源码安装 v_jstools](https://www.cnblogs.com/yoyo1216/p/17601370.html)
- [浏览器过滤网络请求](https://www.freecodecamp.org/chinese/news/chrome-devtools-network-tab-tricks/)
- [js 限制正则表达式长度](https://blog.csdn.net/zjy15203167987/article/details/75024133)
- [fiddler 下载说明](https://juejin.cn/post/7021763777460699150)
- 

## 使用工具
- [v-jstool](https://github.com/cilame/v_jstools)
- [fiddler 下载官网](https://www.telerik.com/download/fiddler)
