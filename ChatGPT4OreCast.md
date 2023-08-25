The ChatGPT technology significantly simplify adoptation of complex systems
using natural language capabilities. We would like to explore adoptation
of ChatGPT for [OreCast](https://github.com/orgs/OreCast/repositories) project.
Users should be able to interact with OreCast services via ChatGPT interface.
The project will invovle the following steps:
- training NN over semi-structured data stored in different OreCast services
- add local version of ChatGPT as interface to OreCast gateway UI
  - train ChatGPT on OreCast meta-data and data
    - the OreCast data will be served via MIN.IO Object Storage and therefore
    will be stored in different format
    - the trained NN model should understand variety of data-format and
    their domains and make appropriate decision to find data based on
    human language
  - a human should place a normal question like:
```
human  > I'm looking for a waste materials which contains certain chemical element

chatGPT > I know a dataset which is located at Site X and can be accessed
via this API. Would like you me to generate you code for its access?
What is your preferred language/platform/infrastructure?

human  > Please generate me a web page which will provide access to this
resource

etc.
```
