<template>
    <div>
        <div>
        <nav class="navbar navbar-expand-lg p-1 navbar-dark bg-success">
        <div class="container-fluid">
            <div class="d-flex align-items-center">
                <img src="/assets/logo.png" width="90" height="80" alt="logo not found" />
                <h1 class="navbar-brand fs-3 ms-3">FINDFORYUMMY</h1>
            </div>
          <button
            class="navbar-toggler"
            type="button"
            data-bs-toggle="collapse"
            data-bs-target="#navbarScroll"
            aria-controls="navbarScroll"
            aria-expanded="false"
            aria-label="Toggle navigation"
          >
            <span class="navbar-toggler-icon"></span>
          </button>
          <div
            class="collapse navbar-collapse navbar-right"
            id="navbarScroll"
          >
            <ul class="navbar-nav ms-auto my-2 my-lg-0 navbar-nav-scroll">
              <li class="nav-item">
                <nuxt-link to="/" class="nav-link active" aria-current="page" href="#">
                  Home
                </nuxt-link>
              </li>
              <li class="nav-item">
                <nuxt-link to="/about" class="nav-link">About</nuxt-link> 
              </li>
              <li class="nav-item">
                <a class="nav-link" href="#">
                  Service
                </a>
              </li>
              <li class="nav-item">
                <a class="nav-link" href="#">
                  Contact
                </a>
              </li>
            </ul>
          </div>
        </div>
      </nav>
    </div>
    <slot></slot>
    </div>
</template>