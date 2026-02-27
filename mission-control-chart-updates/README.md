# Mission Control Chart Dependency Updates

This directory contains the files needed to update the mission-control-chart repository with:
1. Updated dependency versions (config-db and canary-checker)
2. A new workflow to handle automatic dependency updates via repository_dispatch

## Files

- `dependency-update.patch` - Complete patch file with all changes
- `dependency-update.yml` - New workflow file for handling repository_dispatch events
- `chart-Chart.yaml` - Updated chart/Chart.yaml with new dependency versions
- `agent-chart-Chart.yaml` - Updated agent-chart/Chart.yaml with new dependency versions

## Changes Summary

### Dependency Updates
- **config-db**: 0.0.1190 → 0.0.1195
- **canary-checker**: 1.1.3-beta.86 → 1.1.2 (stable release)

### New Workflow
The new `dependency-update.yml` workflow:
- Listens for `repository_dispatch` events with type `dependency-update`
- Automatically updates Chart.yaml files based on the payload from flanksource/charts
- Creates a PR with the dependency updates
- Uses `peter-evans/create-pull-request@v6` for PR creation

## How to Apply

### Option 1: Apply the patch file
```bash
cd /path/to/mission-control-chart
git apply dependency-update.patch
```

### Option 2: Manual application
1. Copy `dependency-update.yml` to `.github/workflows/` in mission-control-chart
2. Copy `chart-Chart.yaml` to `chart/Chart.yaml` in mission-control-chart
3. Copy `agent-chart-Chart.yaml` to `agent-chart/Chart.yaml` in mission-control-chart

## Testing the Integration

Once applied to mission-control-chart, the complete automation flow will be:
1. New chart version pushed to flanksource/charts (gh-pages branch)
2. Charts repository workflow detects the update
3. Charts repository sends repository_dispatch to mission-control-chart
4. Mission-control-chart workflow receives the event and updates dependencies
5. Mission-control-chart workflow creates a PR automatically

## PR Instructions

To create the PR in mission-control-chart with auto-merge:
```bash
cd /path/to/mission-control-chart
git checkout -b update-dependencies
git apply /path/to/dependency-update.patch
git push origin update-dependencies
gh pr create --base main --head update-dependencies --title "Update chart dependencies and add auto-update workflow" --body "See README in mission-control-chart-updates/" --auto-merge --merge-method rebase
```
