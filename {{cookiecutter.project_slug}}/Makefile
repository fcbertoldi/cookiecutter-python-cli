UV_TOOL := $(shell command -v uv)

ifeq ($(UV_TOOL),)
	PIP := .venv/bin/python -m pip
	PIP_COMPILE := .venv/bin/pip-compile
	PIP_SYNC := .venv/bin/pip-sync --pip-args "--no-deps"
	VENV := python -m venv
else
	PIP := VIRTUAL_ENV=.venv $(UV_TOOL) pip
	PIP_COMPILE := VIRTUAL_ENV=.venv $(UV_TOOL) pip compile
	PIP_SYNC := VIRTUAL_ENV=.venv $(UV_TOOL) pip sync
	VENV := $(UV_TOOL) venv
endif

lock: requirements.txt requirements-dev.txt

requirements.txt: requirements.in

requirements-dev.txt: requirements.txt requirements-dev.in

%.txt: %.in
	$(PIP_COMPILE) --generate-hashes --output-file $@ $<

sync:
	$(PIP_SYNC) requirements.txt requirements-dev.txt

build-venv:
	@if [ ! -d ".venv" ]; then \
		$(VENV) .venv; \
		$(PIP_SYNC) requirements.txt requirements-dev.txt; \
		$(PIP) install -e .; \
	fi

build-docker-image:
	docker build -t {{cookiecutter.project_slug}} .

pre-commit-install:
	pre-commit install --install-hooks --overwrite

install:
	pipx install -e .

test: build-venv
	.venv/bin/pytest .

black:
	.venv/bin/black .

ruff-check:
	ruff check .

ruff-fix:
	ruff check --fix .

bandit:
	bandit -c pyproject.toml -r .

clean:
	find . -type d -name "__pycache__" -exec rm -rf {} +
	rm -rf dist
	rm -rf build

.PHONY: sync build-venv test black clean
