---
layout: post
title: Formik -- React form库
excerpt: Formik is a popular option for creating forms in React because it simplifies handling form state, submission, validation, etc.
category: frontend
---

[Tutorial](https://jaredpalmer.com/formik/docs/tutorial)

#### Motivation
react form 写法冗长
1. Getting values in and out of form state
2. Validation and error messages
3. Handling form submission

##### compare with redux

1. 表单状态 form state 是短暂的（ephemeral）和局部的（local），因此无需在Redux（或任何一种Flux库）中对其进行跟踪
2. Redux-Form在每次KEYSTROKE多次调用整个顶级 Redux reducer。这对于小型应用程序来说很好，但是随着Redux应用程序的增长，如果使用Redux Form，输入延迟将会持续增加。
3. Redux-Form压缩后最小为22.5 kB，Formik为12.7 kB。

#### Install

兼容 React v15+ ，可以和 ReactDOM、React Native 一起使用

`npm install formik --save`

`yarn add formik`

```
<script src = "https://unpkg.com/formik/dist/formik.umd.production.js">
// window.Formik.<Insert_Component_Name_Here>
</script>
```

#### The Gist

Formik跟踪表单的状态，然后通过props暴露状态以及一些可重使用的方法和事件处理程序。handleChange和handleBlur完全按预期工作-它们使用name或id属性来确定要更新的字段。

```
import React from 'react';
import { Formik } from 'formik';

const Basic = () => (
  <div>
    <h1>Anywhere in your app!</h1>
    <Formik
      initialValues={{ email: '', password: '' }}
      validate={values => {
        const errors = {};
        if (!values.email) {
          errors.email = 'Required';
        } else if (
          !/^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,}$/i.test(values.email)
        ) {
          errors.email = 'Invalid email address';
        }
        return errors;
      }}
      onSubmit={(values, { setSubmitting }) => {
        setTimeout(() => {
          alert(JSON.stringify(values, null, 2));
          setSubmitting(false);
        }, 400);
      }}
    >
      {({
        values,
        errors,
        touched,
        handleChange,
        handleBlur,
        handleSubmit,
        isSubmitting,
        /* and other goodies */
      }) => (
        <form onSubmit={handleSubmit}>
          <input
            type="email"
            name="email"
            onChange={handleChange}
            onBlur={handleBlur}
            value={values.email}
          />
          {errors.email && touched.email && errors.email}
          <input
            type="password"
            name="password"
            onChange={handleChange}
            onBlur={handleBlur}
            value={values.password}
          />
          {errors.password && touched.password && errors.password}
          <button type="submit" disabled={isSubmitting}>
            Submit
          </button>
        </form>
      )}
    </Formik>
  </div>
);

export default Basic;
```