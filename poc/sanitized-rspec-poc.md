\# Sanitized Proof of Concept



This is a sanitized representation of the test logic used during the research.



It intentionally avoids including private program material, live data, or non-public attachments.



\## Test Logic



```ruby

\# Pseudocode / sanitized structure



describe 'Compliance violation status authorization boundary' do

&#x20; let(:group) { create\_group }

&#x20; let(:project) { create\_project(group: group) }

&#x20; let(:user) { create\_user }



&#x20; let(:role) do

&#x20;   create\_custom\_role(

&#x20;     base\_role: :guest,

&#x20;     abilities: \[:read\_compliance\_dashboard],

&#x20;     namespace: group

&#x20;   )

&#x20; end



&#x20; let(:violation) do

&#x20;   create\_project\_compliance\_violation(

&#x20;     project: project,

&#x20;     namespace: group,

&#x20;     status: :detected

&#x20;   )

&#x20; end



&#x20; before do

&#x20;   assign\_group\_member(

&#x20;     group: group,

&#x20;     user: user,

&#x20;     role: role

&#x20;   )

&#x20; end



&#x20; it 'should not allow a read-only compliance role to update violation status' do

&#x20;   expect(user).to be\_allowed(:read\_compliance\_dashboard)

&#x20;   expect(user).to be\_allowed(:read\_compliance\_violations\_report)



&#x20;   expect(user).not\_to be\_allowed(:admin\_compliance\_framework)



&#x20;   result = execute\_graphql\_mutation\_as(

&#x20;     user: user,

&#x20;     mutation: :update\_project\_compliance\_violation,

&#x20;     variables: {

&#x20;       id: violation.id,

&#x20;       status: 'RESOLVED'

&#x20;     }

&#x20;   )



&#x20;   expect(violation.reload.status).not\_to eq('resolved')

&#x20; end

end

```



\## Expected Result



A user with read-only compliance permissions should not be able to update violation status.



\## Observed Result During Research



The status update succeeded in the local test environment, demonstrating an authorization mismatch.

