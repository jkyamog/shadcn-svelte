---
title: Superforms
description: Building forms with Superforms and Zod.
---

Forms are tricky. They are one of the most common things you'll build in a web application, but also one of the most complex.

Well-designed HTML forms are:

- Well-structured and semantically correct.
- Easy to use and navigate (keyboard).
- Accessible with ARIA attributes and proper labels.
- Has support for client and server side validation.
- Well-styled and consistent with the rest of the application.

In this guide, we will take a look at building forms with [`sveltekit-superforms`](https://superforms.vercel.app/) and [`zod`](https://zod.dev). We're going to use a `<FormField>` component to compose accessible forms using Radix UI components.

## Features

The `<Form />` component is a wrapper around the `superforms` library. It provides a few things:

- Composable components for building forms.
- A `<FormField />` component for building controlled form fields.
- Form validation using `zod`.
- Handles accessibility and error messages.
- Applies the correct `aria` attributes to form fields based on states.
- Built to work with all Radix UI components.
- **You have full control over the markup and styling.**

## Anatomy

```tsx
<Form>
  <FormField
    control={...}
    name="..."
    render={() => (
      <FormItem>
        <FormLabel />
        <FormControl>
          { /* Your form field */}
        </FormControl>
        <FormDescription />
        <FormMessage />
      </FormItem>
    )}
  />
</Form>
```

## Example

```tsx
const form = useForm()

<FormField
  control={form.control}
  name="username"
  render={({ field }) => (
    <FormItem>
      <FormLabel>Username</FormLabel>
      <FormControl>
        <Input placeholder="shadcn" {...field} />
      </FormControl>
      <FormDescription>This is your public display name.</FormDescription>
      <FormMessage />
    </FormItem>
  )}
/>
```

## Installation

1. Install the following dependencies:

```sh
npm install react-hook-form zod @hookform/resolvers @radix-ui/react-slot
```

2. Copy and paste the following `<Form />` component to your app.

<ComponentSource src="/components/react-hook-form/form.tsx" />

<Callout>
  **Note**: You can place this component in a file at `components/ui/form.tsx`
  and import it from there.
</Callout>

## Usage

<Steps>

### Create a form schema

Define the shape of your form using a Zod schema. You can read more about using Zod in the [Zod documentation](https://zod.dev).

```tsx showLineNumbers {4,6-8}
"use client";

import Link from "next/link";
import * as z from "zod";

const formSchema = z.object({
  username: z.string().min(2).max(50)
});
```

### Define a form

Use the `useForm` hook from `react-hook-form` to create a form.

```tsx showLineNumbers {4,14-20,22-27}
"use client";

import { zodResolver } from "@hookform/resolvers/zod";
import Link from "next/link";
import * as z from "zod";

const formSchema = z.object({
  username: z.string().min(2, {
    message: "Username must be at least 2 characters."
  })
});

export function ProfileForm() {
  // 1. Define your form.
  const form = useForm<z.infer<typeof formSchema>>({
    resolver: zodResolver(formSchema),
    defaultValues: {
      username: ""
    }
  });

  // 2. Define a submit handler.
  function onSubmit(values: z.infer<typeof formSchema>) {
    // Do something with the form values.
    // ✅ This will be type-safe and validated.
    console.log(values);
  }
}
```

Since `FormField` is using a controlled component, you need to provide a default value for the field. See the [React Hook Form docs](https://react-hook-form.com/api/usecontroller) to learn more about controlled components.

### Build your form

We can now use the `<Form />` components to build our form.

```tsx showLineNumbers {7-17,28-50}
"use client"

import Link from "next/link"
import { zodResolver } from "@hookform/resolvers/zod"
import * as z from "zod"

import { Button } from "@/components/ui/button"
import {
  Form,
  FormControl,
  FormDescription,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from "@/components/ui/form"
import { Input } from "@/components/ui/input"

const formSchema = z.object({
  username: z.string().min(2, {
    message: "Username must be at least 2 characters.",
  }),
})

export function ProfileForm() {
  // ...

  return (
    <Form {...form}>
      <form onSubmit={form.handleSubmit(onSubmit)} className="space-y-8">
        <FormField
          control={form.control}
          name="username"
          render={({ field }) => (
            <FormItem>
              <FormLabel>Username</FormLabel>
              <FormControl>
                <Input placeholder="shadcn" {...field} />
              </FormControl>
              <FormDescription>
                This is your public display name.
              </FormDescription>
              <FormMessage />
            </FormItem>
          )}
        />
        <Button type="submit">Submit</Button>
      </form>
    </Form>
  )
}
```

### Done

That's it. You now have a fully accessible form that is type-safe with client-side validation.

<ComponentExample
src="/components/examples/input/react-hook-form.tsx"
className="[&\_[role=tablist]]:hidden [&>div>div:first-child]:hidden"

>   <InputReactHookForm />
> </ComponentExample>

</Steps>

## Examples

See the following links for more examples on how to use the `<Form />` component with other components:

- [Checkbox](/docs/components/checkbox#react-hook-form)
- [Date Picker](/docs/components/date-picker#react-hook-form)
- [Input](/docs/components/input#react-hook-form)
- [Radio Group](/docs/components/radio-group#react-hook-form)
- [Select](/docs/components/select#react-hook-form)
- [Switch](/docs/components/switch#react-hook-form)
- [Textarea](/docs/components/textarea#react-hook-form)
- [Combobox](/docs/components/combobox#react-hook-form)