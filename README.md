# impQuestionsReact

#### Form with YUP Validation

import React, { useState } from "react";
import * as yup from "yup";

const validationSchema = yup.object().shape({
  fname: yup.string().required("Name is required!"),
  address: yup.string().required("Address is required!"),
  number: yup
    .number()
    .min(9, "Number should be greater than or equal to 9")
    .required("Number is required!"),
});

function EffectHook() {
  const [formData, setFormData] = useState([]);
  const [formErrors, setFormErrors] = useState({});
  const [user, setUser] = useState({
    fname: "",
    address: "",
    number: "",
  });
  const [showFormData, setShowFormData] = useState(false);

  const handleUser = (e) => {
    const name = e.target.name;
    const value = e.target.value;
    setUser({ ...user, [name]: value });
  };

  const handleSubmit = async (e) => {
    e.preventDefault();

    try {
      await validationSchema.validate(user, { abortEarly: false });

      const formDataObject = { ...user };
      setFormData([...formData, formDataObject]);

      setUser({
        fname: "",
        address: "",
        number: "",
      });

      // Clear form errors
      setFormErrors({});

      // Show form data
      setShowFormData(true);
    } catch (error) {
      if (error instanceof yup.ValidationError) {
        const errors = {};
        error.inner.forEach((e) => {
          errors[e.path] = e.message;
        });
        setFormErrors(errors);

        // Hide form data if there are errors
        setShowFormData(false);
      }
    }
  };

  return (
    <div>
      <form onSubmit={handleSubmit}>
        <h1>Simple Form</h1>
        <div>
          <input
            type="text"
            name="fname"
            id="name"
            value={user.fname}
            onChange={handleUser}
            placeholder="Name"
          />
          <br />
          {formErrors.fname && <p>{formErrors.fname}</p>}
          <input
            type="text"
            name="address"
            id="address"
            value={user.address}
            onChange={handleUser}
            placeholder="Address"
          />
          <br />
          {formErrors.address && <p>{formErrors.address}</p>}
          <input
            type="number"
            name="number"
            id="number"
            min={9}             
            value={user.number}
            onChange={handleUser}
            placeholder="Number"
          />
          <br />
          {formErrors.number && <p>{formErrors.number}</p>}
          <input type="submit" />
        </div>
      </form>
      {showFormData && (
        <>
          <h2>Form Data:</h2>
          <table>
            <thead>
              <tr>
                <th>Name</th>
                <th>Address</th>
                <th>Number</th>
              </tr>
            </thead>
            <tbody>
              {formData.map((data, index) => (
                <tr key={index}>
                  <td>{data.fname}</td>
                  <td>{data.address}</td>
                  <td>{data.number}</td>
                </tr>
              ))}
            </tbody>
          </table>
        </>
      )}
    </div>
  );
}

export default EffectHook;
 
