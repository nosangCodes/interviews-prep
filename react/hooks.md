## useContext

useContext is a react hook that lets you read and subscribe to context any where in the component tree.
context lets the parent component make some data available to its children, it is useful if many components need the same data.

#### Let's see with an example of themeContext

it will help us understand how we can accessa and change the value from the child component

```
    <!-- first we'll create a provider file called "Themeprovider.tsx",  -->

    <!-- default values of the context -->
    type ThemeContextTypes = {
        theme: ThemeTypes;
        onChange: () => void;
    };

    <!-- creating a context -->
    export const ThemeContext = createContext<ThemeContextTypes>({
        theme: "dark",
        onChange: () => {},
    });


    <!-- props type -->
    type Props = {
        children: React.ReactNode;
    };

    export default function ThemeContextProvider({ children }: Props) {
        <!-- logics -->
        const [theme, setTheme] = useState<ThemeTypes>("dark");
        const handleChangeTheme = () => {
            setTheme((prev) => (prev === "dark" ? "light" : "dark"));
        };

        <!-- return the context provider which will be used to wrap children were we want to access the value -->

        <!-- we need to pas the values which we want to make available to the children -->

        return (
            <ThemeContext.Provider value={{ theme, onChange: handleChangeTheme }}>
                {children}
            </ThemeContext.Provider>
    )};

```

#### Now we will wrap the children in the layout file with the ThemeContextprovider

```
    import ThemeContextProvider from "@/components/providers/theme";

    export default function RootLayout({
      children,
    }: {
      children: React.ReactNode;
    }) {
      return (
        <html lang="en">
          <ThemeContextProvider>
            <body>{children}</body>
          </ThemeContextProvider>
        </html>
      );
    }

```

#### lets use the context inside one of the children

This code takes the value and change function from the context we created above and uses it to toggle the div's background color on click of a button.
We can use this to make a dark and light mode website.

```
    <!-- ChildPage.tsx -->
    const { theme, onChange } = useContext(ThemeContext);
    <div
        style={{
          height: "300px",
          width: "300px",
          backgroundColor: theme === "dark" ? "black" : "white",
        }}
        className="flex justify-center items-center transition"
      >
        <button className="border p-2 bg-red-600" onClick={onChange}>
          Change Theme
        </button>
    </div>

```

## useReducer

useReducer is the same as useState in terms of what it does i.e. keep track of state, however
with the help of useReducer we can manage comples state, which is possible with useState but bit overwhelming.

### let's see with the help of an example

first we create a reducer function where we will write the logic to update the state

``` 
    <!--  studentReducer.ts-->

    type StudentStateType = {
        id: number;
        name: string;
        feesPaid: boolean;
    };

    type StudentActionType = {
        type: "has-paid-toggle";
        id: number;
    };

    export function studentReducer(
      state: StudentStateType[],
      action: StudentActionType
    ): StudentStateType[] {
      switch (action.type) {
        case "has-paid-toggle":
          return state.map((student) => {
            if (student.id === action.id) {
              return { ...student, feesPaid: !student.feesPaid };
            } else {
              return student;
            }
          });
      }
    }

```

### let's see the reducer in action with the hrlp of useReducer

```
    <!-- initial data that is being used -->
    export const students: StudentStateType[] = [
    {
        id: 1,
        name: "Student One",
        feesPaid: false,
    },
    {
        id: 2,
        name: "Student two",
        feesPaid: false,
    },
    {
        id: 3,
        name: "Student three",
        feesPaid: false,
    },
    {
        id: 4,
        name: "Student four",
        feesPaid: true,
    },
    ];


    <!-- Students.tsx -->

    <!-- we are getting latest students data and 
    disptach function to update the value -->
    
    const [students, dispatch] = useReducer(studentReducer, studentsData);

    <ul className="flex flex-col gap-y-4 w-fit justify-start mx-auto">
        {students.map((student) => (
            <li className="flex flex-row gap-x-2 items-center" key={student.id}>
            <span>{student.feesPaid ? "💸" : "🙅"}</span>
            <p>{student.name}</p>

            <!-- using dispatch to call the method in the reducer to update students data -->

            {!student.feesPaid && (
                <button
                onClick={() =>
                    dispatch({ type: "has-paid-toggle", id: student.id })
                }
                className="ml-auto border text-xs p-1 bg-emerald-600"
                >
                Mark Paid
                </button>
            )}
            {student.feesPaid && (
                <button
                onClick={() =>
                    dispatch({ type: "has-paid-toggle", id: student.id })
                }
                className=" ml-auto border text-xs p-1 bg-rose-800"
                >
                Revert
                </button>
            )}
            </li>
        ))}
    </ul>

```
