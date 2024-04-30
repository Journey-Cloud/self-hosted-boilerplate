<a name="readme-top"></a>
[![Contributors][contributors-shield]][contributors-url]
[![Forks][forks-shield]][forks-url]
[![Stargazers][stars-shield]][stars-url]
[![Issues][issues-shield]][issues-url]
[![MIT License][license-shield]][license-url]

<!-- PROJECT LOGO -->
<br />
<div align="center">
  <a href="https://github.com/2-App-Studio/journey-sync">
    <img src="https://journey.cloud/home/journey-3d-icon.png" alt="Logo" width="80" height="80">
  </a>

<h3 align="center">Journey Sync</h3>

  <p align="center">
    Journey® is a journal and diary app on mobile and web. Join millions of Journey users, from all walks of life, to embark on your unique life journey towards a deeper gratitude for life, better health, and a calmer mind through journaling.
    <br />
    <br />
    <a href="https://github.com/Journey-Cloud/journey-self-hosted-boilerplate/issues">Report Bug</a>
    ·
    <a href="https://github.com/Journey-Cloud/journey-self-hosted-boilerplate/issues">Request Feature</a>
  </p>
</div>



<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#about-the-project">About The Project</a>
      <ul>
        <li><a href="#built-with">Built With</a></li>
      </ul>
    </li>
    <li>
      <a href="#getting-started">Getting Started</a>
      <ul>
        <li><a href="#prerequisites">Prerequisites</a></li>
        <li><a href="#installation">Installation</a></li>
      </ul>
    </li>
    <li><a href="#usage">Usage</a></li>
    <li><a href="#roadmap">Roadmap</a></li>
    <li><a href="#contributing">Contributing</a></li>
    <li><a href="#license">License</a></li>
    <li><a href="#contact">Contact</a></li>
    <li><a href="#acknowledgments">Acknowledgments</a></li>
  </ol>
</details>



<!-- ABOUT THE PROJECT -->
## About The Project

Journey Sync is meant to be a variant of Journey Cloud that can be easily self-hosted. It offers data privacy and security by allowing you to have full control over your data. 

[![Product Name Screen Shot][product-screenshot]](https://example.com)



<p align="right">(<a href="#readme-top">back to top</a>)</p>



### Built With

* [![Node][Node-logo]][Node-url]
* [![Express][Express-logo]][Express-url]
* [![MongoDB][MongoDB-logo]][MongoDB-url]
* [![Elastic Search][ElasticSearch-logo]][ElasticSearch-url]
* [![RabbitMQ][RabbitMQ-logo]][RabbitMQ-url]

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- GETTING STARTED -->
## Getting Started

This is an example of how you may give instructions on setting up your project locally.
To get a local copy up and running follow these simple example steps.

### Prerequisites

This is an example of how to list things you need to use the software and how to install them.
* Docker
  * Docker is an open platform for developing, shipping, and running applications.
  * [Installation Instructions](https://docs.docker.com/get-docker/)

### Installation

1. Clone the repo
   ```sh
   git clone https://github.com/2-App-Studio/journey-sync.git
   ```
2. Copy environment variables
   ```sh
   cp .env.example .env
   ```
3. Change `ELASTIC_PASSWORD` and `KIBANA_PASSWORD` to secret passwords
   ```env
    ELASTIC_PASSWORD={YOUR_PASSWORD_HERE}
    KIBANA_PASSWORD={YOUR_PASSWORD_HERE}
   ```

3. Start the containers
   ```sh
   npm run start 
   ```

4. There will be a few prompts to set-up your admin account.
  - If you choose to enable End-to-end encryption, make sure you remember your passphrase, as it cannot be recovered.

Note: if you have errors starting up elastic search, you might have to increase your process's memory allocation. Please refer to [the following](https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html#_set_vm_max_map_count_to_at_least_262144) for more information.
  - If you are on Linux, this can be done with:
    ```sh
    sysctl -w vm.max_map_count=262144
    ```

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- USAGE EXAMPLES -->
## Usage

Use this space to show useful examples of how a project can be used. Additional screenshots, code examples and demos work well in this space. You may also link to more resources.

_For more examples, please refer to the [Documentation](https://github.com/2-App-Studio/journey-sync/blob/main/README.md)_

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- ROADMAP -->
## Roadmap

- [ ] Feature 1
- [ ] Feature 2
- [ ] Feature 3
    - [ ] Nested Feature

See the [open issues](https://github.com/2-App-Studio/journey-sync/issues) for a full list of proposed features (and known issues).

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- CONTRIBUTING -->
## Contributing

Contributions are what make the open source community such an amazing place to learn, inspire, and create. Any contributions you make are **greatly appreciated**.

If you have a suggestion that would make this better, please fork the repo and create a pull request. You can also simply open an issue with the tag "enhancement".
Don't forget to give the project a star! Thanks again!

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

### Things to note:
- A separate `docker-compose.dev.yml` is provided for convenient development
  ```sh
  npm run dev 
  ```

  - Includes configurations that support hot reloading
    - `Dockerfile.dev` mounts a volume to the project directory of the host.
    - An `ignore` volume is added to prevent mounting the `node_modules` folder
  - Exposes the MongoDB port for local connections to `http://localhost:27017`.
- The project uses `Joi` for validation of `req.body` and `Mongoose` for operations with `MongoDB`
    - Ideally `Joi` should handle all validations, however, `Mongoose` validations are included as a fallback

- All handlers should avoid sending error status codes as response. Instead, use the `ExternalError` class defined and specify the error code.
    - This is to allow the `errorHandler` middleware to catch all errors, and sanitize to a readable format for the client.

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- LICENSE -->
## License

Distributed under the GNU General Public License. See `LICENSE` for more information.

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- CONTACT -->
## Contact

Project Link: [https://github.com/2-App-Studio/journey-sync](https://github.com/2-App-Studio/journey-sync)

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- ACKNOWLEDGMENTS -->
## Acknowledgments

* []()
* []()
* []()

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->
[contributors-shield]: https://img.shields.io/github/contributors/2-App-Studio/journey-sync.svg?style=for-the-badge
[contributors-url]: https://github.com/2-App-Studio/journey-sync/graphs/contributors
[forks-shield]: https://img.shields.io/github/forks/2-App-Studio/journey-sync.svg?style=for-the-badge
[forks-url]: https://github.com/2-App-Studio/journey-sync/network/members
[stars-shield]: https://img.shields.io/github/stars/2-App-Studio/journey-sync.svg?style=for-the-badge
[stars-url]: https://github.com/2-App-Studio/journey-sync/stargazers
[issues-shield]: https://img.shields.io/github/issues/2-App-Studio/journey-sync.svg?style=for-the-badge
[issues-url]: https://github.com/2-App-Studio/journey-sync/issues
[license-shield]: https://img.shields.io/github/license/2-App-Studio/journey-sync.svg?style=for-the-badge
[license-url]: https://github.com/2-App-Studio/journey-sync/blob/main/LICENSE
[product-screenshot]: images/screenshot.png

[Node-logo]: https://img.shields.io/badge/node.js-333333?style=for-the-badge&logo=nodedotjs&logoColor=5fa04e
[Node-url]: https://nodejs.org/en
[Express-logo]: https://img.shields.io/badge/express-eeeeee?style=for-the-badge&logo=express&logoColor=4a4a4a
[Express-url]: https://expressjs.com/
[MongoDB-logo]: https://img.shields.io/static/v1?style=for-the-badge&message=MongoDB&color=47A248&logo=MongoDB&logoColor=FFFFFF&label=
[MongoDB-url]: https://www.mongodb.com/
[ElasticSearch-logo]: https://img.shields.io/badge/Elastic-FFFFFF?style=for-the-badge&color=005571&logo=Elastic&logoColor=FFFFFF
[ElasticSearch-url]: https://www.elastic.co/
[RabbitMQ-logo]: https://img.shields.io/static/v1?style=for-the-badge&message=RabbitMQ&color=FF6600&logo=RabbitMQ&logoColor=FFFFFF&label=
[RabbitMQ-url]: https://www.rabbitmq.com/
